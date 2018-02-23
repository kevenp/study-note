# mysql分区

## mysql分区方式：
### range分区 （区间分区，连续）

        mysql> CREATE TABLE IF NOT EXISTS `user` (  
        `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '用户ID',  
        `name` varchar(50) NOT NULL DEFAULT '' COMMENT '名称',  
        `sex` int(1) NOT NULL DEFAULT '0' COMMENT '0为男，1为女',  
        PRIMARY KEY (`id`)  
        ) ENGINE=MyISAM  DEFAULT CHARSET=utf8 AUTO_INCREMENT=1  
        PARTITION BY RANGE (id) (  
            PARTITION p0 VALUES LESS THAN (3),  
            PARTITION p1 VALUES LESS THAN (6),  
            PARTITION p2 VALUES LESS THAN (9),  
            PARTITION p3 VALUES LESS THAN (12),  
            PARTITION p4 VALUES LESS THAN MAXVALUE  
        ); 
### list分区 （集合， 不连续，分区时）
   
**如果设置了主键，则分区时，必须已主键分区，否则会失败**

    mysql> CREATE TABLE IF NOT EXISTS `list_part` ( 
      `id` int(11) NOT NULL  COMMENT '用户ID', 
      `province_id` int(2) NOT NULL DEFAULT 0 COMMENT '省', 
      `name` varchar(50) NOT NULL DEFAULT '' COMMENT '名称', 
      `sex` int(1) NOT NULL DEFAULT '0' COMMENT '0为男，1为女'  
    ) ENGINE=INNODB  DEFAULT CHARSET=utf8  
    PARTITION BY LIST (province_id) (  
        PARTITION p0 VALUES IN (1,2,3,4,5,6,7,8),  
        PARTITION p1 VALUES IN (9,10,11,12,16,21),  
        PARTITION p2 VALUES IN (13,14,15,19),  
        PARTITION p3 VALUES IN (17,18,20,22,23,24)  
    ); 

### hash分区

**HASH分区主要用来确保数据在预先确定数目的分区中平均分布，你所要做的只是基于将要被哈希的列值指定一个列值或表达式，以 及指定被分区的表将要被分割成的分区数量** 

    mysql> CREATE TABLE IF NOT EXISTS `hash_part` (  
       `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '评论ID',  
       `comment` varchar(1000) NOT NULL DEFAULT '' COMMENT '评论',  
       `ip` varchar(25) NOT NULL DEFAULT '' COMMENT '来源IP',  
       PRIMARY KEY (`id`)  
     ) ENGINE=INNODB  DEFAULT CHARSET=utf8 AUTO_INCREMENT=1  
     PARTITION BY HASH(id)  
     PARTITIONS 3;  

### key分区

**类似hash分区，使用系统自带的表达式hash**

    mysql> CREATE TABLE IF NOT EXISTS `key_part` (  
       `news_id` int(11) NOT NULL  COMMENT '新闻ID',  
       `content` varchar(1000) NOT NULL DEFAULT '' COMMENT '新闻内容',  
       `u_id` varchar(25) NOT NULL DEFAULT '' COMMENT '来源IP',  
       `create_time` DATE NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '时间'  
     ) ENGINE=INNODB  DEFAULT CHARSET=utf8  
     PARTITION BY LINEAR HASH(YEAR(create_time))  
     PARTITIONS 3;  

### 子分区

子分区是分区表中每个分区的再次分割，子分区既可以使用HASH希分区，也可以使用KEY分区。这 也被称为复合分区（composite partitioning）。

1，如果一个分区中创建了子分区，其他分区也要有子分区

2，如果创建了了分区，每个分区中的子分区数必有相同

3，同一分区内的子分区，名字不相同，不同分区内的子分区名子可以相同（5.1.50不适用）
