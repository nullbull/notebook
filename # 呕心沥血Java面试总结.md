# 老牛Java面试题目总结




### 基础篇
1.  String为什么要设计成Final
2.  String StringBuffer StringBulider的区别
    1.  StringBuffer线程安全，StringBuilder线程不安全
    2.  String final，不可重写      
3.  你能给我写一个final对象吗 
4.  重写hashcode()方法
5.  java [序列化](https://www.cnblogs.com/xdp-gacl/p/3777987.html)
6.  你能给我写个[单例模式](https://www.cnblogs.com/zhaoyan001/p/6365064.html)吗？你这个如果要是被反射或者序列化破坏单例该怎么办？
7.  Java io[流体系结构](https://blog.csdn.net/kingdvc/article/details/42488517)
8.  [BIO、NIO和AIO的区别](https://blog.csdn.net/u013851082/article/details/53942947/)
9.  HashMap的实现原理
10.  HashMap产生冲突了会怎么样
11.  HashMap在多线程下会出现什么问题，为什么会这样
12.  HashMap为什么要采用红黑树，你能给我讲讲红黑树的特点吗
13.  ConcurrentHashMap的实现原理
14.  ConcurrentHashMap JDK1.7和1.8有什么区别
15.  ConcurrenthashMap用到了哪些锁
16.  ArrayList和LinkedList的区别，分别在什么场景下使用
17.  ArrayList的扩容
     1.  负载因子是0.75也就是arraylist的size > length * 0.75便进行扩容
     2.  初始容量是10，扩容为之前的1.5倍
     3.  add 和  remove 都是用的System.arrayCopy;
18.  Vector为什么是线程安全的
19.  TreeSet的底层实现
20.  一些Collection的方法
21.  [Java反射的原理](https://blog.csdn.net/TellH/article/details/70997628)
22.  Servlet初始化流程，filter的处理过程
23.  interface里面接口用什么进行修饰
24.  为什么要先加载父类构造器，如果父类构造器没有无参构造器会怎么办
25.  [接口和抽象类的区别](http://www.importnew.com/12399.html)
     1.  抽象类不能实例化，可以有构造器，成员变量，方法
     2.  抽象类可以通过修饰符来限制适用范围
     3.  static方法不能抽象
     4.  接口方法默认public修饰，变量public static final修饰
     5.  接口多继承，抽象类单继承
     6.  接口中也可以有一些默认方法
     7.  接口中不包含普通方法，抽象类包含
     8.  接口中不能定义普通变量，抽象类可以
     9.  接口不包含构造器，不能用静态代码块。
26.  Java泛型的了解
27.  TreeMap的数据结构



### 并发篇

1. [sychronizated 底层实现](https://blog.csdn.net/javazejian/article/details/72828483)
2. [自旋锁，偏向锁，轻量级锁，重量级锁的介绍以及升级过程]()
3. volatile 底层实现
4. CAS乐观锁的原理
5. ASQ 的原理
6. CountDownLanch 和 CyclicBarrier 的区别和用法
7. [线程池的使用和相关参数](https://blog.csdn.net/pfnie/article/details/52757002)
   1. corePoolSize核心线程池大小
   2. runnableTaskQueue任务队列
      1. LinkedBlockingQueue无界双向链表
      2. SynchronouQueue一个不存储元素的阻塞队列
      3. PriorityBlockingQueue 优先级队列
   3. maximumPoolSize最大线程池大小
   4. ThreadFactory 创建线程的工厂
   5. 饱和策略，当最大线程池满了的情况下采取的策略
      1. AbortPolicy 直接抛出异常
      2. CallerRunsPolicy 只用调用者所在线程来运行任务
      3. DiscardOldestPolicy 丢弃队列中最近的一个任务，并执行当前任务
      4. DiscardPolicy 丢弃这个新加入的任务
   6. keepAliveTime 线程活动保持时间，工作线程空闲后存活多长时间
   7. TimeUnit 线程活动保持时间的单位，可选天，小时，秒毫秒，微秒，纳秒
8. 线程池的拒绝策略
   1. AbortPolicy 直接抛出异常
   2. CallerRunsPolicy 只用调用者所在线程来运行任务
   3. DiscardOldestPolicy 丢弃队列中最近的一个任务，并执行当前任务
   4. DiscardPolicy 丢弃这个新加入的任务
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
21. BlockingQueue知道哪些，PriorityBlockingQueue底层实现
22. ThreadLocal不remove会出现什么问题

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
11. 强引用，软引用，弱引用，虚引用
12. 可达性分析算法
13. [内存泄漏和内存溢出](https://blog.csdn.net/ruiruihahaha/article/details/70270574)

### [类加载](https://www.cnblogs.com/aspirant/p/7200523.html)
1. [双亲委派模型及为什么要用双亲委派模型](https://www.cnblogs.com/wxd0108/p/6681618.html)

2. 类加载器与类的”相同“判断   

3. 类加载器种类 

4. 类加载过程   

5. 自定义类加载器

6. 反射用过么，私有成员变量和私有方法能被反射出来吗

7. 怎样获取一个类的私有方法和私有变量

### 设计模式篇
1. 单例模式
2. 实现生产者消费者模式
3. 单例模式如何防止被破坏
4. 讲一讲适配器模式
5. 观察者模式


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
12. 注解发生在什么时候

### 数据库
1. 数据库隔离级别， 会出现什么问题
2. 数据库索引用过吗？是怎么实现的
3. 索引的最左匹配原则
4. [三大范式](https://blog.csdn.net/k_lawliet/article/details/79552856)
5. 数据库的锁，你能给我介绍下吗
6. 聚簇索引和非聚簇索引
7. [为什么索引要用b+树而不是红黑树，hash表](https://www.cnblogs.com/wade-luffy/p/6292784.html)
8. 你都是怎么优化数据库的
9. [Repeateable-read是怎么解决不可重复读的？幻读是怎么解决的](https://www.cnblogs.com/YFYkuner/p/5178684.html)
10. 数据库的三级封锁协议
11. [innodb 和 myisam 区别](https://www.cnblogs.com/changna1314/p/6878900.html)
    1. InnoDB行级锁，Myisam表级锁
    2. Innodb支持事务，Myisam不支持
    3. Innodb支持外键，Myisam不支持
    4. Innodb索引基于聚簇索引，也就是主键索引
    5. Myisam支持压缩，支持全文索引
    6. Innodb有一个自适应hash索引，当某些索引使用的很频繁时，就会创建一个hash索引
12. 数据库实现排名

### 计算机网络
1. TCP的三次握手和四次握手
2. TCP/IP协议 五层协议的对象头的变化
3. [GET和POST的区别](https://www.cnblogs.com/logsharing/p/8448446.html)
4. [当你输入一个网址/点击一个链接，发生了什么？](https://blog.csdn.net/Random__Walker/article/details/52040146)
5. [拥塞控制](https://blog.csdn.net/u010796790/article/details/52853539)
6. [UDP和TCP区别](https://www.cnblogs.com/xiaomayizoe/p/5258754.html)
7. cookie和session[区别](https://www.cnblogs.com/zlw-xf/p/8001383.html)
8. HTTP/HTTPS的区别
9. [长连接与短链接区别](https://www.jianshu.com/p/caeec6bd1d3f)
## [操作系统](https://blog.csdn.net/gui951753/article/details/79489748)
1. 线程进程区别
   1. 线程是进程的一部分，进程是资源分配的基本单位，线程是CPU执行的基本单位
   2. 进程拥有独立的内存空间，线程共享进程的内存
2. 进程通信方式；
   1. 管道
   2. FIFO(命名管道)
   3. 消息队列
      1. 避免了同步阻塞
      2. 有选择的接受消息
   4. 信号量
   5. 共享内存
   6. 套接字(用于不同机器间的通信)
3. 死锁；
   1. 互斥条件：一个资源一次只能被一个进程使用
   2. 请求和保持条件：一个进程因请求资源而堵塞时，对一伙的资源保持不放
   3. 不剥夺条件：进程获得的资源，在未完全使用完前，不能强制剥夺
   4. 循环等待条件：若干进程之间形成一种首尾相接的环形等待资源关系
4. 常用linux命令；
5. 作业调度算法；
   1. 先来先服务
   2. 短作业优先
   3. 最短剩余时间
   4. 时间片轮转
   5. 优先级调度
6. LRU算法实现
7. 用户态核心态的区别
   1. 用户态-->核心态通过中断，异常等机制进入
   2. 用户态下的城西不能直接访问操作系统的数据结构和程序，当然可以通过中断进入核心态获取
   3. 用户态时，进程能访问的内存空间和对象受限制，获取的CPU可以被抢占，而核心态能够访问所有的内存和数据结构，CPU不能被抢占。
8. 进程上下文切换和线程上下文切换
   1. 线程切换，虚拟内存是相同的，因为都是共享进程的
   2. 进程上下文切换会使之前缓存的内存无效
9. 线程间的通信方式
   1. 互斥量
   2. 事件
   3. 信号量
### 其他

1. maven[生命周期](https://www.cnblogs.com/EasonJim/p/6816340.html)
2. 常用的idea快捷键
3. jsp内置对象
4. http[状态码](http://tool.oschina.net/commons?type=5)
5. http内容

### 手撕算法
1. [十道海量数据处理面试题与十个方法大总结](https://blog.csdn.net/v_JULY_v/article/details/6279498)

2. 递归的时间复杂度

   ![1539568247645](C:\Users\11291\Documents\MarkDown\1539568247645.png)3. 写两个交替执行的线程

   4.  逆序对的归并写法
   5.  两个排序单链表合并
   6.  链表的倒数第N个节点
   7.  TOPN问题
   8.  判断链表是否回文
   9.  判断二叉树是不是平衡二叉树
   10.  求最长回文子串
   11.  求最长公共子串
   12.  LRU实现(LinkedHashMap)
   13.  快速排序
   14.  两个栈模拟队列
   15.  一个数组奇数在前，偶数在后
   16.  生产者消费者
   17.  小数转二进制

### 数据结构

1. 红黑树
2. B树，B+树
3. 字典树

### 其他

 	1. 熟悉什么RPC框架
 	2. 分布式锁
 	3. Redis
 	4. 网络编程



### 一些比较好的博客推荐给大家

1. [大佬1](https://github.com/frank-lam/2019_campus_apply)
2. [大佬2](https://github.com/nullbull/architect-awesome)
3. [大佬](https://github.com/CyC2018/CS-Notes)
4. https://javadoop.com/