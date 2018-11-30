1. `  private void getFieldsName(Class cls, String clazzName, String methodName, Map&lt;String,Integer&gt; paramIndexMap) throws Exception {`
2. ``
3. `        ClassPool pool = ClassPool.getDefault();`
4. `        ClassClassPath classPath = new ClassClassPath(cls);`
5. `        pool.insertClassPath(classPath);`
6. ``
7. `        CtClass cc = pool.get(clazzName);`
8. `        CtMethod cm = cc.getDeclaredMethod(methodName);`
9. `        MethodInfo methodInfo = cm.getMethodInfo();`
10. `        CodeAttribute codeAttribute = methodInfo.getCodeAttribute();`
11. `        LocalVariableAttribute attr = (LocalVariableAttribute) codeAttribute.getAttribute(LocalVariableAttribute.tag);`
12. `        if (attr == null) {`
13. `            return;`
14. `        }`
15. `        int pos = Modifier.isStatic(cm.getModifiers()) ? 0 : 1;`
16. `        for (int i = 0; i &lt; cm.getParameterTypes().length; i++){`
17. `            paramIndexMap.put(attr.variableName(i + pos),i);`
18. `        }`
19. `    }`