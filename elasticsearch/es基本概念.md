# es基本概念

## 索引(index)
相当于表
## 文档(type)
相当于行
## 映射
描述文档数据结构

#指令
> * 192.168.1.41:9200/_cat 会有提示
> * 192.168.1.41:9200/索引/_mapping 显示索引相关信息（type...）

## 批量提交
**_bulk**

    POST /gb/tweet/_bulk

    { "index": { "_id": 1 }}   //下面数据的描述信息
    { "name" : "高性能MySQL", "user_id" : "15", "tweet" : "page's number is gt than 700" , "create_at":"2015-09-25"}
    { "index": { "_id": 2 }}
    { "name" : "Elasticsearch 权威指南", "user_id" : "5", "tweet" : "this is a very nice book" , "create_at":"2018-02-11"}
    { "index": { "_id": 3 }}
    { "name" : "Solr 实战", "user_id" : "1098", "tweet" : "my next book" , "create_at":"2017-06-06"}
    { "index": { "_id": 4 }}
    { "name" : "Kafka 内核分析", "user_id" : "79", "tweet" : "kafka need to compare with rabbitmq" , "create_at":"2016-08-06"}



