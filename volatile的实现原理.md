- volatile的实现原理

1. Lock前缀指令会引起处理器缓存写到内存
2. 一个处理器的缓存写入到内存里使其他处理器的缓存无效



- happen-before的定义

1. 如果一个操作happnes-before另一个操作，那么第一个操作的执行结果将对第二个操作结果可见，而且第一个操作的顺序排在第二个操作之前。
2. 两个操作之间happens-before，并不意味着Java的具体实现必须按照happens-before关系来执行。



- happens-before的规则

1. 程序顺序规则：一个线程中的每个操作，happens-before于该线程中的任意后续操作。
2. 监视器锁规则：对一个锁的解锁，happens-before于随后对这个锁的加锁。
3. volatile变量规则：对一个volatile域的写，happens-before于任意后续对这个volatile域的读。 
   传递性：如果A happens-before B，且B happens-before C，那么A happens-before C。
4. start()规则：如果线程A执行操作ThreadB.start()（启动线程B），那么A线程的ThreadB.start()操作happens-before于线程B中的任意操作。
5. join()规则：如果线程A执行操作ThreadB.join()并成功返回，那么线程B中的任意操作happens-before于线程A从ThreadB.join()操作成功返回。
6. 程序中断规则：对线程interrupted()方法的调用先行于被中断线程的代码检测到中断时间的发生。 
   对象finalize规则：一个对象的初始化完成（构造函数执行结束）先行于发生它的finalize()方法的开始。

- 在Daemnon线程中，不能依靠finally块中的内容来确保执行关闭或清理资源的逻辑 

- Lock和Sychronized优势

  - 可以非堵塞的获取锁
  - 获得锁的线程能够相应中断并释放锁
  - 可以设置获取锁的时间，如果时间到了仍未获取到，则返回。

- 为什么使用线程池

  降低资源消耗

  提高相应速度

  提高线程的可管理性

- submit()可以返回一个future()

- 线程池加入一个线程， 核心线程池满了？将该线程加入等待blockqueue : 加入核心线程池

  blockqueue满了？线程池未满，创建线程：加入blockqueue

- 线程池的shutdown是将所有的线程执行interrupt

- FixedThreadPoolExecutor里面有一个LinkedBlockingQueue，当corePool满了，都插入这个队列中，不会放到maximumPool，所以max没用，keepAliveTime也没用。

- CacheThreadPool里面有SynchronousQueue，这个队列只能容纳一个，corePool为空，maxinumPool没有上限。

- ScheduleThreadPoolExecutor使用DelayQueue



