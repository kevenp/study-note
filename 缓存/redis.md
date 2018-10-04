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

## redis 哨兵/集群
1. sentinel 可以切换主从（2n+1），最少三台，一主二从。如果主redis挂了，可以自动将从redis切换成主redis
2. redis cluster，可以分区，16384 哈希槽，每个节点占一部分区间。可以自动切换主从。

## 集群操作
1. util目录有集群启动工具
2. redis-trip reshard 127.0.0.1:7000 (分配槽点)
3. redis-trip add-node 127.0.0.1:7006 127.0.0.1:7000（添加节点）

## redis 同步机制

    主从同步，从从同步
    psync  部分同步
    bgsave 全量同步

第一次连接或者同步记录不匹配，执行bgsave,将镜像全量同步从redis，同步期间，新的指令放入缓冲，待全量同步完成，再同步缓冲里的指令。当同步复制记录正常时，从redis通过psync同步（和sync相比，支持部分同步）
