

1. 不能用5以上的mysql太sb了各种报错[https://blog.csdn.net/superdangbo/article/details/78732700](https://blog.csdn.net/superdangbo/article/details/78732700 "mysql6.0及以上的版本")Unable to create requested service [org.hibernate.engine.jdbc.env.spi.JdbcEnvironment]

2. jpa 解决org.hibernate.lazyinitializationexception could not initialize proxy - no session [https://www.cnblogs.com/hellxz/p/9037597.html](https://www.cnblogs.com/hellxz/p/9037597.html "sb解决")

3. 关于SpringDataJpa中findOne()方法报错问题[https://blog.csdn.net/lzj4423996/article/details/79437687](https://blog.csdn.net/lzj4423996/article/details/79437687)

4. BeanUtils(被=的Bean, 准备赋值的Bean)

5. 测试类 忘记Runwith@RunWith(SpringRunner.class)
     @SpringBootTest 要不然Bean不装配

6. 注意BeanUtils的次序，先进行赋值，在进行其他赋值

7. 时间戳更新的方法：

   1. 1实体类加注解

   ```
   /**
    * 创建时间
    */
   @CreatedDate
   @Column(name = "create_time")
   private Date createTime;
   
   /**
    * 修改时间
    */
   @LastModifiedDate
   @Column(name = "modify_time")
   private Date modifyTime;
   ```

   1.2.实体类头加注解

   ```
   @EntityListeners(AuditingEntityListener.class)
   ```

   1.3.SpringBoot启动类加注解

   ```
   @EnableJpaAuditing
   ```

   2. 另外数据库添加相应控制也可以：

   `createTime ： CURRENT_TIMESTAMP`
   `modifyTime ： CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`

   3. @DynamicInsert和@DynamicUpdate

   

8. @JsonSerialize 使用@JsonSerialize对javabean进行json格式化

9. @JsonInclude(JsonInclude.Include.NON_NULL) 可以过滤一些为null的属性

10. 全局配置过滤null的属性

   ```
   spring.jackson.default-property-inclusion=non_null
   ```

11. 如何使用自定义注解进行查询

    ```
    @Query(value = "select buyer_openid from order_master o where o.order_id =?1", nativeQuery=true)
    ```

12.  