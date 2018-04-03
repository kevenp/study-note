## redis内存满了怎么办

redis 内存参数maxmemory设置最大内存，内存满了，使用maxmemory-policy来处理后继写入请求。

maxmemory-policy:
|规则名称|规则说明|
|:-|:-|
|noeviction|不删除，返回错误|
|volatile-LRU|使用LRU算法删除键（设置了生存时间）|
|allkeys-LRU|使用LRU算法删除键|
|volatile-random|随机删除一个键（设置了生存时间）|
|allkeys-random|随机删除一个键|
|volatile-ttl|删除生存时间最近的一个键|

**注意**

LRU算法，least Recently Used，最近最少使用算法。也就是说默认删除最近最少使用的键。但是一定要注意一点！redis中并不会准确的删除所有键中最近最少使用的键，而是随机抽取3个键，删除这三个键中最近最少使用的键。那么3这个数字也是可以设置的，对应位置是配置文件中的maxmeory-samples

## redis 数据结构
1. string
2. hash (可存储对象，key对象明，field, value)
3. list (可当做队列，lpush, lrange，key-->(value ...))
4. set（不能重复, key --> (value ...)）
5. sortset(不能重复，key score value)
6. hyperlog(计算总数，有误差)
7. bitmap
8. publish/subscripe
