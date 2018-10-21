1. 查看所有键 keys *
2. 键总数 dbsize
3. 检查键是否存在 exists key
4. 删除键 del key
5. 设置过期时间 expire key 10 
6. 返回键的剩余时间 ttl key
7. 获取key的种类 type key
8. Redis 有五种数据类型：string， hash， list， set， zset
9. Redis 的核心是单线程，为什么这么快
 - 纯内存访问
 - 非堵塞I/O
 - 单线程避免了线程切换和竞态产生的消耗

##   Redis中string数据操作
10. 修改数据 set key xx
11. 添加数据 set key nx
12. 批量设置 key的值 mset
13. 批量获取 key的值 mget
14. 自增 incr key 
15. 自增 n incrby key n
16. 自减 decr key
17. 自减 n decrby key n
18. 像字符串尾添加值 append key value
19. 字符串长度 strlen key
20. 设置并返回原值 getset key value
21. 设置指定位置的字符 setrange key 下标 value
22. 获取指定部分的字符串 get range key start end
23. redis string 最大的值 为512Mb


## Redis中的hash数据操作

0. key -> field -> value
	-> field -> value
	-> field -> value	
	​	一个key 可以对应多个field 
	​	类似List<Map<String, String>> 
25. 设置值 hset key fiel_key value
26. 获取值 hget key fiel_key
27. 删除值 Hdel key fiel_key
28. 批量设置 key的值 hmset
29. 批量获取 key的值 hmget
30. 计算field的个数 hlen key
31. 判断是否存在某个key hexists key
32. 获取key的所有field hkeys key
33. 获取key的所有value hvals key
34. 获取所有的field key hgetall key
35. hash 内部实现有两种 ziplist和hashtable
- 当field个数比较少且没有大的value时，内部编码为ziplist
- 当value大于64字节，内部编码会由ziplist转为hashtable
- 当field个数大于512，编码也会由ziplist转为hashtable


## Redis中List数据操作
36. 从右边插入数据 rpush key value 
37. 从左边插入数据 lpush key value
38. 向某个元素前后插入 linsert key before|after ? value
39. 查找指定范围内的元素列表 lrange key start end
	​	获取全部可以这样： lrange key 0  -1
40. 获取指定下标的元素 lindex key index
41. 获取列表的长度 llend key
42. 左侧删除 lpop key
43. 右侧删除 rpop key
44. 删除指定元素 lrem key count value
	​	这个指令会找到与value的值然后删除，
	​	如果count>0，从左向右删除count个
	​	count=0， 全部删除
	​	count<0,从右向左删除
45. 只保留范围内的值 ltrim key start end
46. 修改指定位置的元素lset key index newValue
47. 
- 当value大于64字节，内部编码会由ziplist转为linkedlist
- 当元素个数大于512，编码也会由ziplist转为linkedlist

##Redis中Set数据操作
48. 添加数据 sadd key x x x
49. 删除元素 srem key x x x
50. 获取key的大小 scard key 
51. 判断元素是否在集合中 sismemeber key x
52. 随机的从集合中返回指定个数元素 srandmemeber key [count]
53. 随机从集合弹出元素 spop key
54. 获取所有元素 smembers key
55. 交集 sinter key1 key2
56. 并集 sunion key1 key2
57. 差集 sdiff key1 key2
58. 将key1, key2交并补得出来的集合保存到一个新的集合： sinterstore destination key  key1 key2
sunionstore destination key  key1 key2
sdiffstore destination key  key1 key2
59. 如果元素都是整数而且个数小于512使用intset否则使用hashtable

##Redis中有序Set数据操作
1. 添加成员 zadd key score element
2. 计算成员个数 zcard key
3. 返回 member的score zscore key member
4. 获取member的 排名，返回一个int member的排名从低到高，zrank key member
5. 获取member的 排名，返回一个int member的排名从高到低，zrevrank key member
6. 删除成员 zrem key member
7. 给成员增加分数 并返回增加后的分数zincrby key increment member 
8. 返回指定排名范围内的成员 zrange / zrevrange key start end [withscores] 
9. 返回指定分数范围内的成员 zrangewithscore / zrevrangewithsoce  min max [withscores]
10. 返回指定分数范围内的个数 zcount key min max
11. 删除指定排名内的 元素zremrangebyrank key start end 
12. 删除指定分数内的 元素 zremrangebyscore key min max
13. 元素个数小于128并且每个元素小于64字节，使用ziplist，否则使用skiplist

##键管理
1. 重命名键 rename key newkey 
2. renamex, 只有newKey不存在时才重命名
3. 随机返回一个键 randomkey
4. 设置过期时间秒为单位expire key time
5. 设置过期时间点秒为单位的时间戳expire key 时间戳
5. 设置过期时间毫秒为单位pexpire key time
6. 删除过期时间 persist 
7. 删除键剩余时长 ttl key
8. 对于字符串类型键，执行set会去除过期时间
9. 不能对元素设置过期时间
10. 迁移数据 migrate 目标地址  key 原子性操作
11. keys 可以遍历键， 但是可能会造成阻塞，一般不用
12. scan 可以解决阻塞问题，是一个O(1)的操作

##慢查询功能
1. slowlog-log-slower-than xxx时间超过xxx的都会被记录下来
slowlog-max-len 设置日志存储大小
2. config set slowlog-log-slower-than xxx
config rewrite 将配置持久化到本地配置文件
3. slowlog get n 获取n条慢查询
4. slowlog len 获取长度
5. slowlog reset 日子重置
6. slow get 将日志存储到mysql等中

##Redis Shell
1. redis-cli -r 3 ping 执行三次ping
2. redis-cli -r 5 -i 1 ping 执行五次，每隔一秒一次
3. redis-server --test-memory 1024检测系统还能给Redis 1G内存
4. redis-benchmark -c 100 -n 20000  100个用户并发一共进行20000次请求 模拟测试
5. redis-cli --latency 检测网络延迟


##Pipeline
1. Pipeline是非原子的
2. Pipeline只能操作一个Redis实例

##事务与Lua
1. Redis 不支持事务回滚
2. multi 开始事务 exec 执行事务
3. watch 确保数据key没有被其他客户端修改，否则不执行
4. eval "return redis.call('get',  KEYS[1] )" 1 hello

##Bitmaps
1. setbit key offset value 设置第offset的值
setbit unique:users:2018-04-21 0 1
2. getbit key offset 获取下标为offset的值
3. bitcount  start end 获取 第几个字节到第几个字节之间的数量



##Redis客户端
1.怎么在windows使用虚拟机中的redis
​	1. 首先要修改redis 的配置文件，找到bind节点，修改bind的节点如下：
​	
​	　　bind的意思是绑定哪个ip地址能够访问服务 ，简单说bind指定的ip才可以访问redis server。
​	　　ps： bind 127.0.0.1 //指定只有本机才能访问redis服务器
​	
	　　　　 bind 0.0.0.0    // 所有的机子都可以访问到redis server
	
	      　　  bind  192.168.1.253  //只有这个ip的机子才可以访问redis server
	
	2. 开放redis的服务端口，也就是在linux上放开6379的端口号
	
	　　1. 打开端口号：iptables -A INPUT -ptcp --dport 端口号（6379） -j ACCEPT
	!!!要安装iptables!!!
	3. 重启刷新端口号：service iptables restart
	 Jedis jedis = new Jedis("192.168.113.130", 6379);
	 jedis.set("hello", "sb");
2. 序列化操作 

``` aspectj
	package com.hisen.web;

import com.hisen.entity.Club;
import io.protostuff.LinkedBuffer;
import io.protostuff.runtime.RuntimeSchema;
import io.protostuff.Schema;
import io.protostuff.ProtobufIOUtil;
public class ProtostuffSerializer {
    private Schema<Club> schema = RuntimeSchema.createFrom(Club.class);
    public byte[] seralize(final  Club club) {
        final LinkedBuffer buffer = LinkedBuffer.allocate(LinkedBuffer.DEFAULT_BUFFER_SIZE);
        try {
            return serializeInternal(club, schema, buffer);
        }catch (final Exception e) {
            throw new IllegalStateException(e.getMessage(), e);
        }finally {
            buffer.clear();
        }
    }
    public Club deseralize(final byte[] bytes) {
        try {
            Club club = deseralizeInternal(bytes, schema.newMessage(), schema);
            if (club != null)
                return club;
        }catch (final Exception e) {
            throw new IllegalStateException(e.getMessage(), e);
        }
        return null;
    }
    @SuppressWarnings("unchecked")
    private <T> byte[] serializeInternal(final  T source, final Schema<T> schema, final LinkedBuffer buffer) {
        return ProtobufIOUtil.toByteArray(source, schema, buffer);
    }
    private <T> T deseralizeInternal(final byte[] bytes, final T result, final Schema<T> schema) {
        ProtobufIOUtil.mergeFrom(bytes, result, schema);
        return result;
    }
}
```
4. Jedis 使用 连接池 


``` mipsasm
      GenericObjectPoolConfig poolConfig = new GenericObjectPoolConfig();
        poolConfig.setMaxTotal(GenericObjectPoolConfig.DEFAULT_MAX_TOTAL * 5);/设置最大连接数默认8
        poolConfig.setMaxIdle(GenericObjectPoolConfig.DEFAULT_MAX_IDLE * 3);//设置最大空闲连接数默认8
        poolConfig.setMinIdle(GenericObjectPoolConfig.DEFAULT_MIN_IDLE * 2);//设置最小空闲连接数默认0
        poolConfig.setJmxEnabled(true);//开启jvx功能
        poolConfig.setMaxWaitMillis(3000);//设置连接池 最大等待时间
        JedisPool jedisPool = new JedisPool("192.168.113.130", 6379);
        Jedis jedis = null;
        try {
            String[] keys = {"hello", "list:1", "mylist"};
            jedis = jedisPool.getResource();
            Pipeline pipeline = jedis.pipelined();
            for (String key : keys)
                pipeline.del(key);
            pipeline.sync();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (jedis != null) jedis.close();
        }
```
5. Pipeline
用pipeline模拟批量删除

``` maxima
       Pipeline pipeline = jedis.pipelined();
            for (String key : keys)
                pipeline.del(key);
            pipeline.sync();
```
syncAndReturnAll()获取操作完成返回的结果
6. Jedis的Lua脚本

``` stylus
        String key = "hello";
        String script = "return redis.call('get', KEYS[1])";
        Object result = jedis.eval(script, 1, key);
```
把脚本加载到redis里，然后执行

``` mipsasm
        String key = "hello";
        String script = "return redis.call('get', KEYS[1])";
        Object result = jedis.eval(script, 1, key);
        String scriptSha = jedis.scriptLoad(script);
        Object result2 = jedis.evalsha(scriptSha, 1, key);
```
## 客户端管理
1.client list 获取所有客户端的信息
qbuf qbuf-free分别代表这个输入缓冲区的总容量和剩余容量
Redis有最大缓存，但是缓冲区不受到限制，但是超过限制可能会出现数据丢失，键值淘汰，OOM等问题
2.info memory 获取使用内存信息
3.info clients 获取最大的client,但不能知道是哪一个
4.固定缓冲区（16KB）字节数组实现， 动态缓冲区使用的列表，动态返回较大的结果 obl代表固定缓冲区的长度，ool代表动态缓冲区列表的长度 omem代表使用的字节数也就是一共用了多少字节
5.age代表连接了多长时间，idle代表最近一次的空闲时间
6.

1.   引入两个配置文件要加上 ignore-unresolvable="true"  <context:property-placeholder ignore-unresolvable="true" location="classpath:redis.properties" />
2.   Spring使用Jedis要注意版本号问题，否则会出现bean无法被实例化

​	





Redis 数据结构

1. SDS简单动态字符串

   struct sdshdr {

   ​	int len;//记录已使用的长度

   ​	int free;//记录未使用的长度

   ​	char buf[]//保存字符串

   }

   优点:

   1. 可以直接获取字符串的长度

    	2. 可以杜绝缓冲区溢出（可能会把别的字符串覆盖掉）
    	3. 可以不用一直申请内存(小于1M，free = len，大于1M， 一共free=30M + len + 1byte)

2. List 和ListNode 是一个双向链表

3. 字典dictht 

   typedef struct dictht {

   ​	  dictEntry ** table //hash数组

   ​	unsigned long size //hash表大小

   ​	unsigned long sizemask //用于计算索引值

   ​	unsigned long used //已经使用的数量

   ​	}dictht

   ![1537668735480](C:\Users\11291\Documents\MarkDown\1537668735480.png)



4. skiplist 跳跃表 最好OlogN最坏 On的查询速度
5. intset整型集合
6. 压缩列表，当list或hash元素为较短的string或int时绘彩通这个数据结构