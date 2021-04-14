1、学习基本用法
2、掌握序列化、反序列化
3、容错，并稍微深入一下exactly-once
4、offset提交，结合kafka的原理深入
5、watermark原理稍微深入学习一下

# Kafka Consumer
- DeserializationSchema
- Start Position Configuration
- Fault Tolerance
- Topic and Partition Discovery
- Offset Committing Behaviour Configuration
- Timestamp Extraction/Watermark Emission
Flink Kafka Consumer 需要知道如何将 Kafka 中的二进制数据转换为 Java 或者 Scala 对象。
https://blog.csdn.net/qq_44962429/article/details/103731379
1、TypeInformationSerializationSchema：
2、JsonDeserializationSchema：将序列化的Json转换为ObjectNode 对象。
3、AvroDeserializationSchema：

# Kafka producer
- The SerializationSchema
- Kafka Producers and Fault Tolerance


# Kafka Connector Metrics
