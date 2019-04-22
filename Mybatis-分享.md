

![参数绑定-第 2 页 (C:\Users\11291\Documents\MarkDown\参数绑定-第 2 页 (1).png)](C:\Users\11291\Downloads\参数绑定-第 2 页 (1).png)

这个类是将Mapper的参数转换成Mybaits 可用的参数的

```java
private static final String GENERIC_NAME_PREFIX = "param";

private final SortedMap<Integer, String> names;

private boolean hasParamAnnotation;


public ParamNameResolver(Configuration config, Method method) {
    final Class<?>[] paramTypes = method.getParameterTypes();//获取参数的类型
    final Annotation[][] paramAnnotations = method.getParameterAnnotations();//获取参数的注解
    final SortedMap<Integer, String> map = new TreeMap<>();
    int paramCount = paramAnnotations.length;//参数的个数 因为一个参数可能有多个注解
    // get names from @Param annotations
    for (int paramIndex = 0; paramIndex < paramCount; paramIndex++) {
        String name = null;
        for (Annotation annotation : paramAnnotations[paramIndex]) {
            if (annotation instanceof Param) {
                hasParamAnnotation = true;
                //如果注解是 @Param name 就是@Param("id") 中的id
                name = ((Param) annotation).value();
                break;
            }
        }
        if (name == null) {
            //如果没有声明@Param
            if (config.isUseActualParamName()) {
                // 那么参数名就是 arg0
                name = getActualParamName(method, paramIndex);
            }
            if (name == null) {
                // use the parameter index as the name ("0", "1", ...)
                // gcode issue #71
                name = String.valueOf(map.size());
            }
        }
        map.put(paramIndex, name);
    }
    names = Collections.unmodifiableSortedMap(map);
}
```

构造函数初始化出一个Map ，key 是数字， value 是参数名

这里用了一个SortedMap 是想保证参数有序



这个方法是返回 包装好的对象

```java
  public Object getNamedParams(Object[] args) {
        final int paramCount = names.size();
        //如果为空 或者为null 直接返回
        if (args == null || paramCount == 0) {
            return null;
            //如果没有 @Param注解 并且 参数只有一个
        } else if (!hasParamAnnotation && paramCount == 1) {
            return args[names.firstKey()];
        } else {
            // 否者都会变成map的形式
            final Map<String, Object> param = new ParamMap<>();
//            用这个HashMap的目的 主要是为了抛异常
            class ParamMap<V> extends HashMap<String, V> {
                private static final long serialVersionUID = -2212268410512043556L;
                @Override
                public V get(Object key) {
                    if (!super.containsKey(key)) {
                        throw new BindingException("Parameter '" + key + "' not found. Available parameters are " + keySet());
                    }
                    return super.get(key);
                }
            }
            int i = 0;
            //这一步是 将参数存两份
            //一份是 用户自定义的 key
            //一份是 Mybatis 生成的key 都是 以param 开头的
            for (Map.Entry<Integer, String> entry : names.entrySet()) {
                param.put(entry.getValue(), args[entry.getKey()]);
                // add generic param names (param1, param2, ...) 添加参数param1 param2
                final String genericParamName = GENERIC_NAME_PREFIX + String.valueOf(i + 1);
                // ensure not to overwrite parameter named with @Param
                if (!names.containsValue(genericParamName)) {
                    param.put(genericParamName, args[entry.getKey()]);
                }
                i++;
            }
            return param;
        }
```

这一部为了生成能够执行的sql

```java
@Override
public BoundSql getBoundSql(Object parameterObject) {
    //这个上下文对象 将 参数通过Ognl的方式 获取
    DynamicContext context = new DynamicContext(configuration, parameterObject);
    rootSqlNode.apply(context);
    // 这一步是构建将 #{username} 替换成 ? ParameterMapping
    // 这时候 sql 已经是 select * from user where id = 1  and username = #{name}
    SqlSourceBuilder sqlSourceParser = new SqlSourceBuilder(configuration);
    Class<?> parameterType = parameterObject == null ? Object.class : parameterObject.getClass();

    //这一部是构建ParamterMapping 同时将#{username} 替换成 ?
    SqlSource sqlSource = sqlSourceParser.parse(context.getSql(), parameterType, context.getBindings());
    BoundSql boundSql = sqlSource.getBoundSql(parameterObject);
    for (Map.Entry<String, Object> entry : context.getBindings().entrySet()) {
        boundSql.setAdditionalParameter(entry.getKey(), entry.getValue());
    }
    return boundSql;
}
```

```java
//这一步 将 ${id} 转换成相应的值
@Override
public boolean apply(DynamicContext context) {
    GenericTokenParser parser = createParser(new BindingTokenParser(context, injectionFilter));
    context.appendSql(parser.parse(text));
    return true;
}

private GenericTokenParser createParser(TokenHandler handler) {
    return new GenericTokenParser("${", "}", handler);
}

private static class BindingTokenParser implements TokenHandler {

    private DynamicContext context;
    private Pattern injectionFilter;

    public BindingTokenParser(DynamicContext context, Pattern injectionFilter) {
        this.context = context;
        this.injectionFilter = injectionFilter;
    }
    // 根据 name 获取到对象的值并替换 原来的${id}
    @Override
    public String handleToken(String content) {
        Object parameter = context.getBindings().get("_parameter");
        if (parameter == null) {
            context.getBindings().put("value", null);
            //这一步 是 判断参数类型是不是普通的 int long byte String 之类的
            //如果是，直接将值放入这个 bingings的Map 中
            //为什么采用这种Ognl的方式，增强扩展性
        } else if (SimpleTypeRegistry.isSimpleType(parameter.getClass())) {
            context.getBindings().put("value", parameter);
        }
        Object value = OgnlCache.getValue(content, context.getBindings());
        String srtValue = value == null ? "" : String.valueOf(value); // issue #274 return "" instead of "null"
        checkInjection(srtValue);
        return srtValue;
    }

    private void checkInjection(String value) {
        if (injectionFilter != null && !injectionFilter.matcher(value).matches()) {
            throw new ScriptingException("Invalid input. Please conform to regex" + injectionFilter.pattern());
        }
    }
}
```

这一部是构建ParamterMapping 同时将#{username} 替换成 ?

```java
public SqlSource parse(String originalSql, Class<?> parameterType, Map<String, Object> additionalParameters) {
    ParameterMappingTokenHandler handler = new ParameterMappingTokenHandler(configuration, parameterType, additionalParameters);
    GenericTokenParser parser = new GenericTokenParser("#{", "}", handler);
    String sql = parser.parse(originalSql);
    return new StaticSqlSource(configuration, sql, handler.getParameterMappings());
}

private static class ParameterMappingTokenHandler extends BaseBuilder implements TokenHandler {

    private List<ParameterMapping> parameterMappings = new ArrayList<>();
    private Class<?> parameterType;
    private MetaObject metaParameters;

    public ParameterMappingTokenHandler(Configuration configuration, Class<?> parameterType, Map<String, Object> additionalParameters) {
        super(configuration);
        this.parameterType = parameterType;
        this.metaParameters = configuration.newMetaObject(additionalParameters);
    }

    public List<ParameterMapping> getParameterMappings() {
        return parameterMappings;
    }

    @Override
    public String handleToken(String content) {
        parameterMappings.add(buildParameterMapping(content));
        return "?";
    }
    // content id,jdbcType=BIGINT
    private ParameterMapping buildParameterMapping(String content) {
        //现在这个map 就是
        // property -> id, jdbc -> BIGINT
        Map<String, String> propertiesMap = parseParameterMapping(content);
        String property = propertiesMap.get("property");
        Class<?> propertyType;
        if (metaParameters.hasGetter(property)) { // issue #448 get type from additional params
            propertyType = metaParameters.getGetterType(property);
        } else if (typeHandlerRegistry.hasTypeHandler(parameterType)) {
            propertyType = parameterType;
        } else if (JdbcType.CURSOR.name().equals(propertiesMap.get("jdbcType"))) {
            propertyType = java.sql.ResultSet.class;
        } else if (property == null || Map.class.isAssignableFrom(parameterType)) {
            propertyType = Object.class;
        } else {
            MetaClass metaClass = MetaClass.forClass(parameterType, configuration.getReflectorFactory());
            if (metaClass.hasGetter(property)) {
                propertyType = metaClass.getGetterType(property);
            } else {
                propertyType = Object.class;
            }
        }
        ParameterMapping.Builder builder = new ParameterMapping.Builder(configuration, property, propertyType);
        Class<?> javaType = propertyType;
        String typeHandlerAlias = null;
        for (Map.Entry<String, String> entry : propertiesMap.entrySet()) {
            String name = entry.getKey();
            String value = entry.getValue();
            //根据map 中的key， 找到相应的TypeHandler
            if ("javaType".equals(name)) {
                javaType = resolveClass(value);
                builder.javaType(javaType);
            } else if ("jdbcType".equals(name)) {
                builder.jdbcType(resolveJdbcType(value));
            } else if ("mode".equals(name)) {
                builder.mode(resolveParameterMode(value));
            } else if ("numericScale".equals(name)) {
                builder.numericScale(Integer.valueOf(value));
            } else if ("resultMap".equals(name)) {
                builder.resultMapId(value);
            } else if ("typeHandler".equals(name)) {
                typeHandlerAlias = value;
            } else if ("jdbcTypeName".equals(name)) {
                builder.jdbcTypeName(value);
            } else if ("property".equals(name)) {
                // Do Nothing
            } else if ("expression".equals(name)) {
                throw new BuilderException("Expression based parameters are not supported yet");
            } else {
                throw new BuilderException("An invalid property '" + name + "' was found in mapping #{" + content + "}.  Valid properties are " + parameterProperties);
            }
        }
        if (typeHandlerAlias != null) {
            builder.typeHandler(resolveTypeHandler(javaType, typeHandlerAlias));
        }
        return builder.build();
    }

    private Map<String, String> parseParameterMapping(String content) {
        try {
            return new ParameterExpression(content);
        } catch (BuilderException ex) {
            throw ex;
        } catch (Exception ex) {
            throw new BuilderException("Parsing error was found in mapping #{" + content + "}.  Check syntax #{property|(expression), var1=value1, var2=value2, ...} ", ex);
        }
    }
}
```

```java
@Override
public void setParameters(PreparedStatement ps) {
    ErrorContext.instance().activity("setting parameters").object(mappedStatement.getParameterMap().getId());
    List<ParameterMapping> parameterMappings = boundSql.getParameterMappings();
    if (parameterMappings != null) {
        for (int i = 0; i < parameterMappings.size(); i++) {
            ParameterMapping parameterMapping = parameterMappings.get(i);
            if (parameterMapping.getMode() != ParameterMode.OUT) {
                Object value;
                String propertyName = parameterMapping.getProperty();
                if (boundSql.hasAdditionalParameter(propertyName)) { // issue #448 ask first for additional params
                    value = boundSql.getAdditionalParameter(propertyName);
                } else if (parameterObject == null) {
                    value = null;
                } else if (typeHandlerRegistry.hasTypeHandler(parameterObject.getClass())) {
                    value = parameterObject;
                } else {
                    MetaObject metaObject = configuration.newMetaObject(parameterObject);
                    value = metaObject.getValue(propertyName);
                }
                TypeHandler typeHandler = parameterMapping.getTypeHandler();
                JdbcType jdbcType = parameterMapping.getJdbcType();
                if (value == null && jdbcType == null) {
                    jdbcType = configuration.getJdbcTypeForNull();
                }
                try {
                    typeHandler.setParameter(ps, i + 1, value, jdbcType);
                } catch (TypeException e) {
                    throw new TypeException("Could not set parameters for mapping: " + parameterMapping + ". Cause: " + e, e);
                } catch (SQLException e) {
                    throw new TypeException("Could not set parameters for mapping: " + parameterMapping + ". Cause: " + e, e);
                }
            }
        }
    }
}
```

最后一步就是 将参数对应的值 添加到PreparedStatement 中

```java
@Override
public void setParameters(PreparedStatement ps) {
    //  比如说现在有一个User 参数 user: id=1 username=zwt password=123456 sex=1
    ErrorContext.instance().activity("setting parameters").object(mappedStatement.getParameterMap().getId());
    List<ParameterMapping> parameterMappings = boundSql.getParameterMappings();
    //parameterMapping  property = id jdbcTypeHandler  jdbcType = BIGINT typeHandler LongTypeHandler
    if (parameterMappings != null) {
        for (int i = 0; i < parameterMappings.size(); i++) {
            ParameterMapping parameterMapping = parameterMappings.get(i);
            if (parameterMapping.getMode() != ParameterMode.OUT) {
                Object value;
                String propertyName = parameterMapping.getProperty();
                if (boundSql.hasAdditionalParameter(propertyName)) { // issue #448 ask first for additional params
                    value = boundSql.getAdditionalParameter(propertyName);
                } else if (parameterObject == null) {
                    value = null;
                } else if (typeHandlerRegistry.hasTypeHandler(parameterObject.getClass())) {
                    value = parameterObject;
                } else {
                    //根据User 生成一个MetaObject 对象 MetaObject 对User 进行解析
                    // 可以解析出所有的set get 方法，然后通过反射的方式获取到值
                    MetaObject metaObject = configuration.newMetaObject(parameterObject);
                    value = metaObject.getValue(propertyName);
                }
                //根据ParameterMapping 中的TypeHandler 将值进行处理
                TypeHandler typeHandler = parameterMapping.getTypeHandler();
                JdbcType jdbcType = parameterMapping.getJdbcType();
                if (value == null && jdbcType == null) {
                    jdbcType = configuration.getJdbcTypeForNull();
                }
                try {
                    typeHandler.setParameter(ps, i + 1, value, jdbcType);
                } catch (TypeException e) {
                    throw new TypeException("Could not set parameters for mapping: " + parameterMapping + ". Cause: " + e, e);
                } catch (SQLException e) {
                    throw new TypeException("Could not set parameters for mapping: " + parameterMapping + ". Cause: " + e, e);
                }
            }
        }
    }
}
```

