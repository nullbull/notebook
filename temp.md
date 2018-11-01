Signed-off-by: niuzhenhao <niuzhenhao@xiaomi.com>



### 抢单设计

1. 系统启动，将当天的Express加入到Redis，设置过期时间一天
2. 订单更新，将之前的key过期，插入一条新的
3. 有人抢单，添加分布式锁，将该key过期
4. mq更新mysql，创建订单

### 秒杀设计

1. 系统启动，将商品信息加入redis

2. 加Redis分布式锁，对Redis数量进行更新，如果小于0返回失败，如果大于0进行下一步

3. 将消息发送到mq，更新mysql，创建订单

4. 出现的问题：

   1. 如何保证发送成功

   2. 消费者失败了怎么办

### [分布式事物](https://www.cnblogs.com/savorboard/p/distributed-system-transaction-consistency.html)

1. CAP原则，不可能同时满足
   1. 一致性Consistentcy
   2. 可用性 Availablility
   3. 分区容错性 Partition tolerance
2. BASE理论
   1. 基本可用 Basically Available
   2. 软状态Soft State
   3. 最终一致性 Eventually consistent
3. 解决方案
   1. 两段式提交2PC
   2. TCC 补偿事物
   3. 本地消息表
   4. RocketMQ



<<<<<<< HEAD

=======
缓存和Mysql的策略

1. 读操作
>>>>>>> ac7c1a9a1abc530c247e69236ab430433f6145ae

SOA面向服务的架构

```
# 配置github.com
Host github.com                 
    HostName github.com
    IdentityFile /home/justinniu/.ssh/github_id_rsa
    PreferredAuthentications publickey
    User justinniu

# 配置 gerrit.pt.miui.com	
Host gerrit.pt.miui.com	 
    HostName git.oschina.net
    IdentityFile /home/justinniu/.ssh/id_rsa
    PreferredAuthentications publickey
    User niuzhenhao
```

<<<<<<< HEAD


### ThreadLocal

ThreadLocal 里面有个ThreadLocalMap,这个Map是线程私有的,所以ThreadLocal只是给这个Treadlocal赋值,因为可能这个线程有多个Threadlocal对象,所以这个map以当前ThreadLocal为key,把set的对象作为value

ThreadLocalMap key是弱引用,也就是ThreadLocal被回收之后, 在下一次gc时,这个key就会成为null, 虽然在get/set操作时会删除key为null的Entry,但是还是有可能在很长时间内null, Object的Entry没有被回收,直到该线程被回收,才能被gc回收.

如果使用场景是线程池,如果不remove, get方法能访问到之前set的值.所以在线程池慎用ThreadLocal



![1541052831366](/home/justinniu/MarkDown/notebook/1541052831366.png)
