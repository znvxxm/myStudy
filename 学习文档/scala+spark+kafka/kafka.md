topic:逻辑上的概念
partition：物理上的概念,每个分区又进行了索引和分片操作.index和.log
日志分区
日志地址配置文件：./etc/kafka/conf.dist/server.properties


#创建topic
-  bin/kafka-topics --create --zookeeper 10.45.157.131:2181 --replication-factor 1 --partitions 5 --topic kafka-test
#删除topic
-  bin/kafka-topics --delete --zookeeper 10.45.157.131:2181 --topic test
#查看topic
-  bin/kafka-topics --list --zookeeper 10.45.157.131:2181
#查看某个topic详细信息
-  bin/kafka-topics --zookeeper 10.45.157.131:2181 --describe --topic test 
#发送消息
-  bin/kafka-console-producer --broker-list 10.45.157.131:9092 --topic test
#消费消息
-  bin/kafka-console-consumer --bootstrap-server 10.45.157.131:9092 --topic test --from-beginning

#创建消费者组
bin/kafka-console-consumer --bootstrap-server 10.45.157.131:9092 --topic kafka-test --consumer-property group.id=kafka-test-group

#将消费者组内offset置为0，从头开始消费
kafka-consumer-groups --bootstrap-server lv131.dct-znv.com:9092 --group kafka-test-group --topic bdp_quality_verify_data --reset-offsets --to-earliest --execute

##查看消费者组
-旧版本
 bin/kafka-consumer-groups --list --zookeeper 10.45.157.131:2181 
- 新版本
 bin/kafka-consumer-groups --new-consumer --bootstrap-server 10.45.157.131:9092 --list
 
 #查看特定消费者组详情
 bin/kafka-consumer-groups --new-consumer --bootstrap-server 10.45.157.131:9092 --group kafka-test-group --describe

##producer生产者
**分区策略
  --指定分区写入
  --根据哈希值写入
  --不指定k值，采用轮询算法写入分区
**可靠性机制（ack）
ack决定数据丢不丢失，ack=-1保证不丢失，不保证重复；ack=0，保证不重复，不保证丢失
ISR?

##kafka API

序列化器必须要有，分区器和拦截器可选，均在properies里面添加
#生产者API，producer API






 