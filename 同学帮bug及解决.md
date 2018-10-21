1. Thymeleaf  + Shiro 出现下面的问题`java.lang.NoClassDefFoundError: org/thymeleaf/dialect/AbstractDialect`

   添加依赖，要用springboot1.5，使用2.0还会报错

```
<thymeleaf-layout-dialect.version>2.2.2</thymeleaf-layout-dialect.version>
```

```
<dependency>
    <groupId>com.github.theborakompanioni</groupId>
    <artifactId>thymeleaf-extras-shiro</artifactId>
    <version>2.0.0</version>
</dependency>
```

```
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf</artifactId>
    <version>3.0.9.RELEASE</version>
</dependency>
```

2. Controller 方法使用 返回值 String 渲染不出来来页面是因为没有添加

   ```
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-thymeleaf</artifactId>
   </dependency
   ```

如果pom文件报错，说明和之前的meven下载文件冲突了，建议把所有.m2/下载文件删除，重新下载

3. `No message found under code 'productname.required' for locale 'null'.`说明国际化配置文件出现了问题，没有配置好，访问不到该配置文件的code，需要在yml添加

   ```
   spring:
     messages:
         #国际化资源文件路径
       basename: i18n/messages
   ```

一定要控制好yml文件的格式，不然会无效，验证方法是ctrl+B能够访问到



4. 为什么我登陆了之后没有走过滤器，我知道为什么了，就像xml配置一样，配置类也需要对需要对路径绑定相应的过滤器才能发挥作用，所以加上这一条，过滤器就能发挥作用，而这个过滤器的最用就是去给予一个id，另一个过滤器根据这个id判断是否需要存储到数据库

   ```java
   filterChainDefinitionMap.put("/hello", "onlineSession,syncOnlineSession");
   ```

5. 写了一个工具类，可以把一些常用类型如果为null，封装成对应的初始值，但是忘了一点switch要break

6. 使用Mybatis Example 返回的List 一定不为null,需要判断其是否为空

7. 数据库中desc describe 这些字段都是 数据库保存的

8. mybatis 没使用 驼峰命名法

9. 阿里云开放端口，使用外部连接之后，在linux本机就没法使用producer生产消息了