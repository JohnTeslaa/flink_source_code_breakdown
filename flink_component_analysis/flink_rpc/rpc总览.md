来自官网：https://ci.apache.org/projects/flink/flink-docs-release-1.12/deployment/config.html#rpc--akka
Flink uses Akka for RPC between components (JobManager/TaskManager/ResourceManager). Flink does not use Akka for data transport.

https://flink.apache.org/2019/06/05/flink-network-stack.html
https://zhuanlan.zhihu.com/p/70827849 中文翻译
In contrast to the coordination channels between TaskManagers and JobManagers which are using RPCs via Akka, the network stack between TaskManagers relies on a much lower-level API using Netty.



flink_rpc的整体实现流程
待完成RPC各个组件的源码阅读后，参考这篇文章，做一个整体架构的梳理 20210313 TODO
https://www.cnblogs.com/leesf456/p/11120045.html


RPC provider:


RPC comsumer:


## 启动RpcEndpoint的流程
当启动RpcServer后，即创建了相应的Actor（注意此时Actor的处于停止状态）和动态代理对象，需要调用RpcEndpoint#start启动启动Actor，此时启动RpcEndpoint流程如下（以非FencedRpcEndpoint为例）：

1、调用RpcEndpoint#start；

2、委托给RpcServer#start；

3、调用动态代理的AkkaInvocationHandler#invoke；发现调用的是StartStoppable#start方法，则直接进行本地方法调用；invoke方法的代码如下：

4、调用AkkaInvocationHandler#start；

5、通过ActorRef#tell给对应的Actor发送消息rpcEndpoint.tell(ControlMessages.START, ActorRef.noSender());；

6、调用AkkaRpcActor#handleControlMessage处理控制类型消息；

7、在主线程中将自身状态变更为Started状态；

经过上述步骤就完成了Actor的启动过程，Actor启动后便可与Acto通信让其执行代码（如runSync/callSync等）和处理Rpc请求了。

* 与Actor通信，通过调用runSync/callSync等方法其直接执行代码。

* 当调用非AkkaInvocationHandler实现的方法时，则进行Rpc请求。

