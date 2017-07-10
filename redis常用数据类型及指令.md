# Redis数据结构及命令

redis的数据结构分为字符串、列表、集合、散列、有序集合

## **一、字符串**
**字符串可以存储以下三种类型** <br>

* 字节串
* 整数
* 浮点数

字符串常用命令

1. set key value
2. get key
3. incr key 值加一 如果key不存在，则value=0;下同
4. decr key 减一
5. incrby key amount 加上amount
6. decrby key amount 减去amount
7. incrbyfloat key f 加上浮点数f
8. append key 拼接
9. getrange key startposition endposition

## **二、列表**

常用命令

1. rpush
2. lpush
3. rpop
4. lpop
5. lindex
6. lrange 全范围是 0 -1
7. ltrim
8. blpop 阻塞
9. brpop
10. rpoplpush 从一个列表移动到另一个列表
11. brpoplpush 阻塞式移动

## **三、集合**

集合常用命令

1. sadd key item
2. srem key item 删除整个集合
3. sismember key item 检查元素item是否存在集合里面
4. scard key 返回集合中包含的元素数量
5. smembers key 返回所有元素
6. srandmember key count 随机返回count个元素 可以为负数
7. spop 随机移出一个元素
8. smove source-key dest-key item 移动到新集合
9. sdiff key-name [key-name ...] 返回第一个集合中存在但其他集合中不存在的元素 差集
10. sdiffstore dest-key key-name [key-name ...] 将差集存到新的集合
11. sinter key-name [key-name] 交集
12. sinterstore dest-key key-name [key-name ...] 交集并存储
13. sunion key-name [key-name ...]并集
14. sunionstore dest-key key-name [key-name ...] 并集并存储

## **四、散列**

散列常用命令

1. hmget key-name key [key ...] 获取多个键的值
2. hmset key-name key value [key value ...]
3. hdel key-name key [key ...]
4. hlen key-name 获取散列包含的键值对数量
5. hexsits key-name key 检查给定键是否存在于散列中
6. hkeys key-name
7. hvals key-name
8. hgetall key-name 获取散列中所有键值对
9. hincrby key-name key increment
10. hincrbyfloat key-name key increment

## **五、有序集合**

有序集合存储着成员与分值之间的映射，类似于散列

常用命令如下

1. zadd key-name score member [score member ...]
2. zrem key-name member [member ...]
3. zcard key-name 返回键所对应的成员数量
4. zincrby key-name increment member 将成员的分值加上increment
5. zcount key-name min max 返回分值介于之间的数量
6. zrank key-name member 返回member的排名
7. zscore key-name member 返回member的分值
8. zrange key-name start stop 返回成员
9. zinterstore dest-key key-count key [key ...] 交集运算，可指定聚合函数 sum min max
10. zunionstore dest-key key-count key [key ...] 并集运算，可指定聚合函数 sum min max

## **六、其他指令**

1. sort source-key [by pattern] [limit offset count] [get pattern [get pattern ...]]<br> [asc|desc] [alpha] [store dest-key]

	```
	
		>>>hset d-7 field 5
		>hset d-15 field 1
		>hset d-23 field 9
		>hset d-110 field 3
		>rpush sort-input 23 15 110 7
		>sort sort-input by d-*->field get d-*->field 根据散列权重排名
	```

2. redis的事务
	* 事务
	
		```

			Transaction multi = conn.multi();
			mulit.incr("key");
			mulit.decr("key");
			mulit.exec();
		```
	* 流水线（管道）
		
		```

			Pipeline pipeline = conn.pipelined();
			pipeline.incr("key");
			pipeline.decr("key");
			pipeline.exec();
		```