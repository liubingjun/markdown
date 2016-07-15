---
title: Redis 概念
date: 2016-07-15 20:31:25
tags: Redis
---
## Redis数据类型
### 字符串
Redis 字符串是一个字节序列。在 Redis 中字符串是二进制安全的，这意味着它们没有任何特殊终端字符来确定长度，所以可以存储任何长度为 512 兆的字符串。  
```
redis 127.0.0.1:6379> SET name "yiibai"
OK
redis 127.0.0.1:6379> GET name
"yiibai"
```  
在上面的例子中，SET 和 GET 是 Redis 命令，name 和 "yiibai" 是存储在 Redis 的键和字符串值。  
### 哈希
Redis哈希是键值对的集合。 Redis哈希是字符串字段和字符串值之间的映射，所以它们用来表示对象。  
```  
redis 127.0.0.1:6379> HMSET user:1 username yiibai password yiibai points 200
OK
redis 127.0.0.1:6379> HGETALL user:1
1) "username"
2) "yiibai"
3) "password"
4) "yiibai"
5) "points"
6) "200"
```  
在上面的例子中，哈希数据类型用于存储包含用户基本信息的用户对象。这里 HSET，HEXTALL 是 Redis 命令同时 user:1 也是一个键。
### 列表
Redis 列表是简单的字符串列表，通过插入顺序排序。可以添加一个元素到 Redis 列表的头部或尾部。  
```  
redis 127.0.0.1:6379> lpush tutoriallist redis
(integer) 1
redis 127.0.0.1:6379> lpush tutoriallist mongodb
(integer) 2
redis 127.0.0.1:6379> lpush tutoriallist rabitmq
(integer) 3
redis 127.0.0.1:6379> lrange tutoriallist 0 10
1) "rabitmq"
2) "mongodb"
3) "redis"
```  
列表的最大长度为  232 - 1 个元素（4294967295，每个列表的元素超过四十亿）。
### 集合
Redis 集合是字符串的无序集合。在 Redis 可以添加，删除和测试成员存在的时间复杂度为 O（1）。  
```  
redis 127.0.0.1:6379> sadd tutoriallist redis
(integer) 1
redis 127.0.0.1:6379> sadd tutoriallist mongodb
(integer) 1
redis 127.0.0.1:6379> sadd tutoriallist rabitmq
(integer) 1
redis 127.0.0.1:6379> sadd tutoriallist rabitmq
(integer) 0
redis 127.0.0.1:6379> smembers tutoriallist
1) "rabitmq"
2) "mongodb"
3) "redis"
```  
注：在上面的例子中 rabitmq 被添加两次，但由于它是只集合具有唯一特性。集合中的成员最大数量为 232 - 1（4294967295，每个集合有超过四十亿条记录）。
### 集合排序
不同的是，一个有序集合的每个成员都可以排序，就是为了按有序集合排序获取它们，按权重分值从最小到最大排序。虽然成员都是独一无二的，按权重分数值可能会重复。  
```  
redis 127.0.0.1:6379> zadd tutoriallist 0 redis
(integer) 1
redis 127.0.0.1:6379> zadd tutoriallist 0 mongodb
(integer) 1
redis 127.0.0.1:6379> zadd tutoriallist 0 rabitmq
(integer) 1
redis 127.0.0.1:6379> zadd tutoriallist 0 rabitmq
(integer) 0
redis 127.0.0.1:6379> ZRANGEBYSCORE tutoriallist 0 1000
1) "redis"
2) "mongodb"
3) "rabitmq"
```  
***
## Redis分区
分区是将数据分割成多个 Redis 实例，使每个实例将只包含键子集的过程。
### 分区的好处
它允许更大的数据库，使用多台计算机的内存总和。如果不分区，只是一台计算机有限的内存可以支持的数据存储；
它允许按比例在多内核和多个计算机计算，以及网络带宽向多台计算机和网络适配器；
### 分区的劣势
涉及多个键的操作通常不支持。例如，如果它们被存储在被映射到不同的 Redis 实例键，则不能在两个集合之间执行交集；
涉及多个键时，Redis事务无法使用；
分区粒度是一个键，所以它不可能使用一个键和一个非常大的有序集合分享一个数据集；
当使用分区，数据处理比较复杂，比如要处理多个RDB/AOF文件，使数据备份需要从多个实例和主机聚集持久性文件；
添加和删除的容量可能会很复杂。例如：Redis的Cluster支持数据在运行时添加和删除节点是透明平衡的，但其他系统，如客户端的分区和代理服务器不支持此功能
### 分区类型
Redis 提供有两种类型的分区。假设我们有四个 redis 实例：R0，R1，R2，R3，分别表示用户用户如：user:1, user:2, ...等等
### 范围分区
范围分区被映射对象指定 Redis 实例在一个范围内完成。
在我们的例子中，用户从ID为 0 至 ID10000 将进入实例 R0，而用户 ID 10001到ID 20000 将进入实例 R1 等等。
### 散列分区
在这种类型的分区是一个散列函数（例如，模数函数）用于将键转换为数字数据，然后存储在不同的 redis 实例。
