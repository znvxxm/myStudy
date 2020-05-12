//kafkaTopicDS只是起了一个是sparkstreaming流，但是还没有进行RDD操作。需要进行RDD的相关操作后才能打印数据


 //从kafka中读取的消息，是用ConsumerRecord类进行消费的
 //ConsummerRecord有两个构造函数，包含了消息的主题，分区，偏移量，秘钥K，消息本体value
 //1.ConsumerRecord(String topic, int partition, long offset, K key, V value)
 //2.ConsumerRecord(String topic, int partition, long offset, long timestamp, org.apache.kafka.common.record.TimestampType timestampType, long checksum, int serializedKeySize, int serializedValueSize, K key, V value)

  //读取kafka中消费者记录中的消息，要用到value()方法将其取出
    kafkaTopicDS.map(key=>key.value).print()