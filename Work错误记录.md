1. SpringBoot Controller 如果返回String 要用Model， 要是用ModelAndView，就得返回ModelAndView，要不然就和打开html页面没什么区别

2. 在spring-shiro.xml中配置shiroFilter， 

   ```
   <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
       <property name="securityManager" ref="securityManager" />
       <property name="loginUrl" value="/login/login" />
       <property name="successUrl" value="/comm/list" />
       <property name="unauthorizedUrl" value="/login/login" />
       <property name="filterChainDefinitions">
           <value>
               <!--/api/**=anon-->
               <!--/res/**=anon-->
               <!--/src/**=anon-->
               <!--/health/**=anon-->
               <!--/logout=authc-->
               <!--/openid=anon-->
               <!--/callback=anon-->
               <!--/=authc-->
               <!--/**=anon-->
               /pur/**=role[user]
               /comm/**=perms[admin:manage]
               /login/**=anon
           </value>
       </property>
   </bean>
   ```

perms匹配的是角色的权限，

roles匹配的是特定的角色

authc匹配的是用户必须登陆

具体详情见：https://www.cnblogs.com/rxl007/p/5787433.html



3. JavaMailSender出现 553 mail from must equal authorized user，应该是邮箱太新，不能发送带有html的内容。是发送内容错误造成的错误。
4. 今天遇到了空指针错误，原来就是忘记把Bean纳入Spring容器管理。
5. 要发送邮件带附件需要

`MimeMessageHelper mimeMessageHelper = new MimeMessageHelper(mimeMessage, true);`

配置true；

6. Cause: java.lang.ClassCastException: com.github.pagehelper.PageHelper cannot be cast to org.apache.ibatis.plugin.Interceptor 这里要用com.github.pagehelper.PageInterceptor不能用com.github.pagehelper.PageHelper

![1531718436970](C:\Users\11291\AppData\Local\Temp\1531718436970.png)

7. thymeleaf判断是否为空

                        `<h2 th:text="${session.currentUser==null} ? '11' : ${session.currentUser.getUserName()}"></h2>`  

8. 用反射实现从excel读取的数据转换成实体类
9. 

