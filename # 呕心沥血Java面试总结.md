# 老牛Java面试题目总结




### 基础篇
1.  String为什么要设计成Final
2. String StringBuffer StringBulider的区别      
3. 你能给我写一个final对象吗 
4. 重写hashcode()方法
### 集合篇
1. HashMap的实现原理
2. HashMap产生冲突了会怎么样
3. HashMap在多线程下会出现什么问题，为什么会这样
4. HashMap为什么要采用红黑树，你能给我讲讲红黑树的特点吗
5. ConcurrentHashMap的实现原理
6. ConcurrentHashMap JDK1.7和1.8有什么区别
7. ConcurrenthashMap用到了哪些锁
8. ArrayList和LinkedList的区别，分别在什么场景下使用
9. ArrayList的扩容
10. Vector为什么是线程安全的
11. TreeSet的底层实现
12. 一些Collection的方法

### 并发篇
1. [sychronizated 底层实现](https://blog.csdn.net/javazejian/article/details/72828483)
2. [自旋锁，偏向锁，轻量级锁，重量级锁的介绍以及升级过程]()
3. volatile 底层实现
4. CAS乐观锁的原理
5. ASQ 的原理
6. CountDownLanch 和 CyclicBarrier 的区别和用法
7. [线程池的使用和相关参数](https://blog.csdn.net/pfnie/article/details/52757002)
8. 线程池的拒绝策略
9. FixedThreadPool SingleThreadPool CacheThreadPool 一些细节，比如说用了什么队列，空闲线程的等待时间等等。
10. sleep和wait的区别
11. notify和notifyAll的区别
12. 如何实现线程按顺序执行
13. 为什么wait, notify, notifyAll方法定义在Object里
14. 你是怎样理解线程安全的
15. [sychronizated使用在方法上和sychronizated(xxx.class)和sychroizated(this)有什么区别](https://blog.csdn.net/luckey_zh/article/details/53815694)
16. ThreadLocal用过吗，给我介绍下他的使用场景
17. Lock和Sychronizated的区别
18. Callable和Future了解过吗？
19. 为什么说ConcurrentHashMap是弱一致性的？以及为何多个线程并发修改ConcurrentHashMap时不会报ConcurrentModificationException？
20. 线程运行的状态以及如何他们之间是怎么切换的

### 底层篇
1. JVM 内存模型
2. JVM 垃圾回收算法
3. JVM 垃圾回收器
4. JVM major gc 和 full gc 的触发时机
5. [CMS的过程为什么要标记两次](https://blog.csdn.net/zqz_zqz/article/details/70568819)
6. new一个对象会放在哪里
7. JVM 调整的一些参数
8. 怎么判定一个对象的内存可以被回收了
9. 哪些对象可以作为CGRoots
10. [什么情况下会发生内存泄露](https://blog.csdn.net/morningsun1990/article/details/25456707)
11. 强引用，弱引用，虚引用
12. 可达性分析算法

### [类加载](https://www.cnblogs.com/aspirant/p/7200523.html)
1. 双亲委派模型及为什么要用双亲委派模型
2. 什么是类加载器    
3. 类加载器与类的”相同“判断   
4. 类加载器种类 
5. 类加载过程   
6. 自定义类加载器
7. 反射用过么，私有成员变量和私有方法能被反射出来吗
8. 怎样获取一个类的私有方法和私有变量

### Spring篇
1. [SpringIOC初始化过程](https://blog.csdn.net/zqh994828/article/details/78452421)
2. BeanFactory和ApplicationContext的区别
3. [Spring Bean的初始化]()
4. [Spring AOP的实现原理](https://www.ibm.com/developerworks/cn/java/j-lo-springaopcglib/)
5. [JDK 和CGLIB的区别](https://blog.csdn.net/dreamrealised/article/details/12885739)
6. Spring 和SpringBoot的区别
7. [Spring事务传播机制](https://blog.csdn.net/hcmony/article/details/77850183)
8. SpringMvc的请求过程
9. SpringMvc用到的设计模式
10. 你用过的一些Spring注解
11. 如何让Spring bean按顺序初始化

### 数据库
1. 数据库隔离级别， 会出现什么问题
2. 数据库索引用过吗？是怎么实现的
3. 索引的最左匹配原则
4. 三大范式
5. 数据库的锁，你能给我介绍下吗
6. 聚簇索引和非聚簇索引
7. [为什么索引要用b+树而不是红黑树，hash表](https://www.cnblogs.com/wade-luffy/p/6292784.html)
8. 你都是怎么优化数据库的



一些比较好的博客推荐给大家

1. [大佬1](https://github.com/frank-lam/2019_campus_apply)
2.  [大佬2](https://github.com/nullbull/architect-awesome)
3. [大佬](https://github.com/CyC2018/CS-Notes)