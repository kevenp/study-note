## MYSQL 常见疑难

### 查询慢（哪怕只有一行）
#### 查询长时间不返回
1. MDL锁    可查询`sys.schema_table_lock_waits`,`kill blocking_pid`
2. 等flush  直接查询`show processlist`
3. 行锁     可查询`sys.innodb_lock_waits`

#### 查询慢

    多个事务交叉执行（当前读，一致读）

### 数据库备份

    数据库的备份，在引擎不支持事务的情况下可使用`FTWRL` (`flush table with read lock`)。`FTWRL`是全局锁，用在主数据库，业务不能写，用在从库，备份不能写。
    全局读还有另一种方式：set global readonly=true。但是这种方式不适合用来做全局锁。原因有三
    
    1. readonly属性有可能被用来表示主从库，用这个设置全局锁，影响较大。
    2. 在异常处理上，`FTWRL` 执行后，客户端发生异常断开，mysql会自动释放全局锁。readonly方式，就算客户端异常断开，mysql仍会保持readonly状态，导致数据库不可写
    3. readonly方式，对root权限用户无效，即root用户仍然可写

### mysql 加锁规则（RR隔离级别）

1. 原则1：加锁的基本单位是 next-key lock。前开后闭区间
2. 原则2：查找过程中访问到的对象才会加锁。
3. 优化1：索引上的等值查询，给唯一索引加锁的时候，next-key lock退化为行锁。
4. 优化2：索引上的等值查询，享有遍历时且最后一个值不满足等值条件的时候， next-key lock 退化为间隙锁
5. 一个bug：唯一索引上的范围查询会访问到不满足条件的第一个值为止