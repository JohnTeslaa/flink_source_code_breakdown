Flink采用Akka作为RPC通信的组件

akka 是为了通信，不适合大数据量的传输，像hadoop flink hbase 这些后面都用netty 来做节点间数据的传输

![image](https://user-images.githubusercontent.com/42859030/110233371-8205be80-7f5e-11eb-82b9-2ec35e7961df.png)
