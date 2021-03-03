Flink采用Akka作为RPC通信的组件

akka 是为了通信，不适合大数据量的传输，像hadoop flink hbase 这些后面都用netty 来做节点间数据的传输

