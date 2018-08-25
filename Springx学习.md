1. @Primary 提高自动装配Bean的优先级
2. 在自动装配上面声明
@Qualifier("xxxx") 指定想要自动匹配的Bean
3. 在想要自动装配的Bean上声明@Component
4. @ComponentScan("soundSystem") 在一个配置类中只能写一个要不然在Junit测试是就会在@ContextConfiguration(classes = CDplayerConfig.class)报错
5. xxxxxConfig 貌似不能使用@Autowird 来自动生成bean， 只能显示的@Bean来 
6. 这么多错误，原来只是少了一个包 需要aspectjrt-1.8.xx.jar 和aspectjweaver.jar
7.@Rule
    public final StandardOutputStreamLog log = new StandardOutputStreamLog();
	这个会把输出记录到日志里
8. Aop的XML配置

		    <aop:config>
		        <aop:pointcut id="performance" expression="execution(* Spring.AOP.Performance.perform(..))">
		        </aop:pointcut>//定义切点
		        <aop:aspect ref="audience">
		            <aop:before pointcut-ref="performance" method="silenceCellPhones"/>
		            <aop:around method="watchPerformance" pointcut-ref="performance"/>
		            <aop:before method="takeSeats" pointcut="execution(* Spring.AOP.Performance.perform())"/>
		            <aop:after-returning method="applause" pointcut="execution(* Spring.AOP.Performance.perform())"/>
		            <aop:after-throwing method="demandRefund" pointcut="execution(* Spring.AOP.Performance.perform())"/>
		        </aop:aspect>
		    </aop:config>


10. XML的适用范围：当被调用的Bean不能添加注释的时候
11. 

		<bean id="trackCounter"
		class="soundsystem.TrackCounter" />	
		<bean id="cd"
		class="soundsystem.BlankDisc">
			<property name="title"
			value="Sgt. Pepper's Lonely Hearts Club Band" />
			<property name="artist" value="The Beatles" />
			<property name="tracks">
				<list>
					<value>Sgt. Pepper's Lonely Hearts Club Band</value>
					<value>With a Little Help from My Friends</value>
					<value>Lucy in the Sky with Diamonds</value>
					<value>Getting Better</value>
					<value>Fixing a Hole</value>
					<!-- ...other tracks omitted for brevity... -->
				</list>
			</property>
		</bean>
		<aop:config>
			<aop:aspect ref="trackCounter">
			<aop:pointcut id="trackPlayed" expression=
			"execution(* soundsystem.CompactDisc.playTrack(int))
			and args(trackNumber)" />

			<aop:before
			pointcut-ref="trackPlayed"
			method="countTrack"/>
		</aop:aspect>
		</aop:config>
		</beans>

11. AspectJ运行失败
12. `org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer` 
以前是在spring.jar里的，不过现在Spring MVC已经作为一个单独的了项目拿出来了， 发布时没有加到SpringFrameWork里面， 这部分在spring-webmvc.jar这个jar中
13. AbstractAnnotationConfigDispatcherServletInitializer会同时创
建DispatcherServlet和ContextLoaderListener。 GetServletConfigClasses()方法返回的带有@Configuration注解的类将会用来定
义DispatcherServlet应用上下文中的bean。 getRootConfigClasses()方法返回的
带有@Configuration注解的类将会用来配置ContextLoaderListener创建的应用上下
文中的bean
13. 弄了半天在配置好的SSH环境下运行SpringMVC真是太傻了
14. 注释和applicationContext.xml还有一个冲突。
15. 找了找别人的博客，终于把测试环境解决了
16. javax.validation.* 里面的Error errors没有hasErrors()方法
用BindingResult的hasErrors()方法；
 import org.springframework.validation.BindingResult;
17. bean 在依赖注入完之后才开始初始化的
18. BeanPostProcessor
19. 方案1

		public List<XXXBean> getXXXBeanList(String xxId, String xxCode);  
		
		<select id="getXXXBeanList" resultType="XXBean">
		
		　　select t.* from tableName where id = #{0} and name = #{1}  
		
		</select>  

由于是多参数那么就不能使用parameterType， 改用#｛index｝是第几个就用第几个的索引，索引从0开始
方案2（推荐）基于注解
		
		public List<XXXBean> getXXXBeanList(@Param("id")String id, @Param("code")String code);  
		
		<select id="getXXXBeanList" resultType="XXBean">
		
		　　select t.* from tableName where id = #{id} and name = #{code}  
		
		</select>  




13.         @Component("student")
			        public class Student {
			            /**
			             * 初始化方法
			             */
			            @PostConstruct
			            public void init(){
			                System.out.println("init");
			            }
			            
			            /**
			             * 销毁方法
			             */
			            @PreDestroy
			            public void destory(){
			                System.out.println("destory");
			            }
			        }
14. 获取application.properties的值
 1. Environment.getProperty()
 2. @Value("${xx:default}")
 3. setDefaultProperties()

15. 添加新的配置文件
	@PropertySource("classpath:application.properties")
16. 设置前缀读取，直接可以匹配并赋值 @ConfigurationProperties(prefix = "ds") 要设置get和set 方法

17. EnvironmentPostProcessor读取配置文件 需要挺麻烦的配置
	public class MyEnvironmentPostProcessor implements EnvironmentPostProcessor {
	    @Override
	    public void postProcessEnvironment(ConfigurableEnvironment environment, SpringApplication application) {
	        try(InputStream inputStream = new FileInputStream("d:/test/test.properties")) {
	            Properties properties = new Properties();
	            properties.load(inputStream);
	            PropertiesPropertySource propertySource = new PropertiesPropertySource("my", properties);
	            environment.getPropertySources().addLast(propertySource);
	        }catch (IOException e) {
	            e.printStackTrace();
	        }
	    }
	}
18. 读取后缀为xx的胚子文件，同时默认的配置文件内容也可以获取         application.setAdditionalProfiles("test");
同时，也只有设置了    @Profile() 的bean 被读取 默认的也不能被读取
19. condition
	1. @ConditionOnProperty
	2. @ConditionOnClass
	3. @ConditionOnBean
20. @Import 可以引入bean， 也可以引入配置类， 还可以是ImportSelector 和 ImportBeanDeifintionRegistor 的子类  原理是ImportSelector， 返回含有类名的String[]数组，然后纳入到bean的管理，而ImportBe'an'DifintionRegistor 直接把bean注册到Spring容器中
21. @EnableAutoConfiguration 可以引入 外部的包和bean和配置
22. 
   1. 定义事件 extends ApplicationEvent
   2. 定义监听器 extends ApplicationListener
   3.  1. 添加监听器 addListeners() 
     2. 可以将监听器纳入到Spring容器管理@Component
     3. 在配置文件中 添加context.listener.classes= (详细可见DelegatingApplicationListener)
     4. 不定义监听器 使用 @EventListener，将其纳入到Spring管理
   4. 发布事件 publishEvent（）
23. ApplicationContextInitializer 在Spring refresh之前的一个回掉   springApplication.addInitializers();
24.在resource下创建banner.txt可以在控制台输出

25. 查一下SpringBoot的启动方式
26. 怎么样在SpringMVC中使用Servlet，Filter， Listener
27. 1. @ServletCompant 
	2. ServletRegisterBean
28. 写一个拦截器 继承HandlerInterceptor接口，然后写一个类，继承WebMvcConfigurerAdaptor，重写addInterceptors方法
29. ErrorPageRegistrar重写错误界面
30. ExceptionHandler 处理 这个Controller下面的异常
31. zookeeper， shiro