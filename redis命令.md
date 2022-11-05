## Redis

	后台启动redis
		redis-server redis.conf
	查看是否启动
		netstat -lanp|grep 6379        →6379为redis默认端口
			结果 tcp   0   0 0.0.0.0:6379    0.0.0.0:*      LISTEN      34330/redis-server  
	    或 ps -ef|grep redis
			结果 root 972  1  0 21:52 ?    00:00:01 /usr/local/redis/bin/redis-server 127.0.0.1:6379    ←
			  root      31939   2933  0 22:14 pts/0    00:00:00 grep --color=auto redis
	关闭redis
		kill -9 972

#### 使用redis自带客户端

​	1.
​		redis-cli [options] [commonds]
​		

		常见options :
			1.  -h 127.0.0.1 : 指定要连接的redis节点的ip地址,默认是127.0.0.1
			2.  -p 6379 :  指定要连接的redis节点的端口,默认是6379
			3.  -a redis : 指定redis的访问密码
	
		commonds : redis的操作命令
			ping : 与redis服务器做心跳测试,服务器会正常返回pong
	
		不指定commonds时,会进入 redis-cli的交互融合

### Redis命令

#### 	通用命令

##### KEYS: 查看符合模板的所有的key

​		通过help [command]可以查看一个命令的具体用法

DEL: 删除一个指定的key

EXISTS:判断一个key是否存在

EXPIRE: 给一个key设置有效期,有效期到期时自动删除该key

TTL:查看一个key的剩余有效期 

​	-2:被删除

​	-1:永久保存

> #### String类型命令
>

​		value是字符串,根据格式不同分为3类:

​			string:普通字符串

​			int:整数类型,可以做自增自减操作  二进制

​			float:浮点类型,可以做自增自减操作

​	**底层都是字节数组形式储存,只不过编码方式不同;字符串类型的最大空间不能超过512m**

> ​	命令:

- SET:添加或者修改一个已经存在的String类型的键值对

- GET:根据key获取String类型的value

- MSET:批量添加多个String类型的键值对

- MGET:根据多个key获取多个String类型的value

- INCR:让一个整形的key自增1

- INCRBY:让一个整形的key自增并指定步长,

  ​	例如: incrby num 2 让num值自增2

- INCRBYFLOAT:让一个浮点型的数字自增并指定步长
- SETNX:添加一个String类型的键值对,前提是这个key不存在,否则不执行
- SETEX:添加一个String类型的键值对,并且指定有效期

> #### Hash类型命令

​	hash类型,也叫散列,其中value是一个无序字典,类似于java中的hashMap结构

​	String结构是将对象序列化为JSON字符串后存储,当需要修改某个字段时很不方便:

![屏幕截图 2022-11-03 120738](F:\主文件夹\文档\笔记\截图\屏幕截图 2022-11-03 120738.png)

​	Hash结构可以将对象中的每个字段独立存储,可以针对单个字段做CRUD

​	![屏幕截图 2022-11-03 120812](F:\主文件夹\文档\笔记\截图\屏幕截图 2022-11-03 120812.png)



> **常见命令**

- HSET key field value :添加或者修改hash类型的field的值
- HGET key field:获取一个hash类型的key的值
- HMSET:批量添加多个hash类型key的field的值
- HMGET:批量获取多个hash类型的key的field的值
- HGETALL:获取一个hash烈性的key中的所有的field和value
- HKEYS:获取一个hash类型的key中的所有的field
- HVALS:获取一个hash类型的key中所有的value
- HINCRBY:让一个hash类型key字段值自增并且指定步长
- HSETNX:添加一个hash类型的key的field值,前提是这个field不存在,否则不执行

#### List类型命令

​	Redis中的List类型和java中的LinkedList类似,可以看做是一个双向链表结构,既可以支持正向检索,也可以支持反向检索

- > ​	**特征**(**常用来存储一个有序数据,如朋友圈点赞,评论列表等**)

  - 有序
  - 元素可以重复
  - 插入和删除快
  - 查询速度一般

- > **List类型的常见命令**

  - LPUSH key element ... : 向列表左侧插入一个或多个元素
  - LPOP key : 移除并返回列表左侧的第一个元素,没有则返回nil
  - RPUSH key element ... : 向列表右侧插入一个或多个元素
  - RPOP key : 移除并返回列表右侧的第一个元素
  - LRANGE key star end : 返回一段角标范围内的所有元素
  - BLPOP和BRPOP : 与LPOP和RPOP类似,只不过在没有元素时等待指定时间,而不是直接返回nil

​	**tips**

![image-20221103124437635](C:\Users\13475\AppData\Roaming\Typora\typora-user-images\image-20221103124437635.png)

#### Set类型命令

​	Redis的Set结构与java中的HashSet类似,可以看做是一个value为null的hashMap.是一个hash表

- > **具备与HashSet类似的特征**

  - 无序
  - 元素不可重复
  - 查找快
  - 支持交集、并集、差集等功能

> - **Set类型的常见命令**

- String的常见命令

  - 单个集合

  - SADD key member ... : 向set中添加一个或多个元素
  - SREM key member ... : 移除set中指定元素
  - SCARD key : 返回set中元素的个数
  - SISMEMBER key member : 判断一个元素是否存在于set中
  - SMEMBERS : 获取set中所有的元素
  - 多个集合
  - SINTER key1 key2 ... : 求key1与key2的交集 (key1和key2都有的元素)
  - SDIFF key1 key2 ... : 求key1与key2的差集 (key1有但key2没有的元素)
  - SUNION key1 key2 ... : 求key1与key2的并集 (key1和key2中所有不重复的元素) *只填一个key时相当于查询这个key下的所有元素*

#### SortedSet类型命令

​	Redis的SortedSet是一个可排序的set集合,与java中的TreeSet有些类似,但底层数据结构差别很大。SortSet中的每一个元素都带有一个score属性,可以基于score属性对元素排序,底层的实现是一个跳表(SkipList)加hash表。

- > SortedSet特性: 
  >
  > *因为可排序特性,经常被用来实现排行榜这样的功能*

  - 可排序
  - 元素不可重复
  - 查询速度快

- > **SortedSet类型的常见命令**  
  >
  > *所有的排名默认都是升序,如果要降序则在命令的Z后面添加REV即可*

  - ZADD key score member : 添加一个或多个元素到SortedSet,如果已经存在则更新其score值
  - ZREM key member : 删除SortedSet中的一个指定元素
  - ZSCORE key member : 获取SortedSet中的指定元素的score值
  - ZRANK key member : 获取SortedSet中的指定元素的排名
  - ZCARD key : 获取SortedSet中的元素个数
  - ZCOUNT key : 统计score值在给定范围内的所有元素的个数
  - ZINCRBY key increment member : 让SortedSet中的指定元素自增,步长为指定的increment值
  - ZRANGE key min max : 按照score排序后,获取指定排名范围内的元素
  - ZRANGEBYSCORE key min max : 按照score排序后,获取指定score范围内的元素
  - ZDIFF、ZINTER、ZUNION : 求差集、交集、并集

springboot整合redis

*注意创建redis配置文件的位置,要在springboot启动文件的同级或以下*

不然会报错Could not autowire. No beans of ‘XXXX‘ type found