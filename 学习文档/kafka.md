topic:
日志分区


#创建topic
-  bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
#查看topic
-  bin/kafka-topics --list --zookeeper localhost:2181
#查看某个topic详细信息
-  bin/kafka-topics --zookeeper localhost:2181 --describe --topic test 
#发送消息
-  bin/kafka-console-producer --broker-list localhost:9092 --topic test
#消费消息
-  bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning


