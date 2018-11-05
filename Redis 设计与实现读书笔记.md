# Redis 数据结构

### SDS简单动态字符串

```c
struct sdshdr {

	int len;//记录已使用的长度

	int free;//记录未使用的长度

	char buf[]//保存字符串

}
```

优点:

1. 可以直接获取字符串的长度,O(1)复杂度
2. 可以杜绝缓冲区溢出（可能会把别的字符串覆盖掉）,先扩容,在修改
3. 扩容会申请额外的内存空间,如果SDS的长度小于1M,那么扩容为以前的2倍,如果大于1M,额外添加1M的额外内存
4. 如果Free空间够,则先使用Free空间,否则就申请额外空间
5. 惰性释放,如果SDS缩短,那么不会立刻释放内存,而是增长Free空间
6. SDS通过Len判断字符串是否结束

### List

```c
typedef struct listNode {	

	listNode *prev;

	listNode *next;

	void *value;

}listNode;

typedef struct list{

	listNode *head;

	listNode *tail;

	unsigned  long len;

}list;
```



1. List 是一个双向链表
2. 无环, head.prev tail.next 都为null
3. 存储长度
4. 列表key,发布与订阅,慢查询, 监视器

### 字典dictht 

```c
typedef struct dictht {

	dictEntry ** table; //hash数组

	unsigned long size;//hash表大小

	unsigned long sizemask; //用于计算索引值

	unsigned long used ;//已经使用的数量

}dictht
```

```c
typedef struct dictEntry {

	void *key;//键

	union{//值

		void *val;

		uint64_t u64;

		int64_t s64;

	} v;

	struct dictEntry * next;

}dictEntry
```



1. skiplist 跳跃表 最好OlogN最坏 On的查询速度
2. intset整型集合
3. 压缩列表，当list或hash元素为较短的string或int时绘彩通这个数据结构
4. 拉链法解决hash冲突
5. 渐进式rehash
6. 用于数据库和哈希key

### [跳跃表](https://blog.csdn.net/lz710117239/article/details/78408919)

1. 用于Zset有序集合和在集群结点中做内部结构
2. 平均O(logN),最差O(N)的查询速度
3. ![img](https://upload.wikimedia.org/wikipedia/commons/2/2c/Skip_list_add_element-en.gif)

![Skip_list_add_element-en](/home/justinniu/download/Skip_list_add_element-en.gif)

### 整数集合

```c
typedef struct intset {

	uint32_t encoding;//编码格式

	uint32_t length;//长度

	int8_t contents[];//数组

}intset;
```

1. 如果添加的int大于当前的数组的长度,会进行数组升级,也就是改变编码格式.
2. 如果发生升级,那么最大的数就在数组尾. 
3. 通过升级更省内存,也更灵活

### 压缩列表ZipList

1. 如果列表Key只包含少量的列表项,并且每个元素都是小整数或者短的字符串,就会采用压缩表,同样的,当Key和Value 也符合上述条件时,也会用压缩列表实现Hash
2. 数据结构
   1. zlbytes 压缩列表的字节长度
   2. zltail 尾结点的头 的偏移量
   3. zllen 记录数组的长度
   4. entry
3. 结点的数据结构
   1. previous_entry_length 前一数组的字节长度
   2. encoding  前几位表示 是数组还是整数后几位代表长度
   3. content 可以是一个字节数组或者整数
   4. 可能会出现需要连锁更新的情况

### Redis中的对象

```c
typedef struct redisObject {

	usigned type; //数据类型

	unsigned encoding; //具体的编码格式

	void *ptr; //指向 存储的内存地址

};
```

不同类型和编码的对象

| 类型         | 编码                      | 对象                               |
| ------------ | ------------------------- | ---------------------------------- |
| redis_string | redis_encoding_int        | 整数字符串                         |
| redis_string | redis_encoding_embstr     | embstr编码的简单动态字符串         |
| redis_string | redis_encoding_raw        | 简单动态字符串实现的字符串对象     |
| redis_list   | redis_encoding_ziplist    | 使用压缩列表实现的列表对象         |
| redis_list   | redis_encoding_linkedlist | 使用双向链表实现的列表对象         |
| redis_hash   | redis_encoding_ziplist    | 使用压缩列表实现的哈希对象         |
| redis_hash   | redis_encoding_ht         | 使用哈希表实现的哈希对象           |
| redis_set    | redis_encoding_intset     | 使用整数集合实现的集合对象         |
| redis_set    | redis_encoding_ht         | 使用字典实现的集合对象             |
| redis_zset   | redis_encoding_ziplist    | 使用跳跃表实现的有序集合对象       |
| redis_zset   | redis_encoding_skiplist   | 使用跳跃表和字典实现的有序集合对象 |

