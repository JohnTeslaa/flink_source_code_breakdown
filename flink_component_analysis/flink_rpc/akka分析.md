# 概览
Flink采用Akka作为RPC通信的组件

akka 是为了通信，不适合大数据量的传输，像hadoop flink hbase 这些后面都用netty 来做节点间数据的传输


# FencedRpcEndpoint

## 原理背景：
Fencing token:一种资源保护机制。（fenced：围起来的 -> 即隔离机制。）
机制原理：
https://www.jianshu.com/p/73512e7730fd
![image](https://user-images.githubusercontent.com/42859030/110233371-8205be80-7f5e-11eb-82b9-2ec35e7961df.png)

参见：Hadoop的fencing机制
https://www.cnblogs.com/lushilin/p/11239908.html

## FencedRpcEndpoint原理
给RpcEndpint增加了一个fencing-token，这是一个连续增长的整数(fencing-token的本意)，FencedRpcEndpoint只处理那些**附带的fencing-token跟自己的fencing-token一样的消息**，这个功能明确地指定**让某个actor处理某个消息**。

## 重要的方法
