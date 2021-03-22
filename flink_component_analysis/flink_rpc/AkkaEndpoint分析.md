# 概览
Flink采用Akka作为RPC通信的组件.

akka 是为了通信，不适合大数据量的传输，像hadoop flink hbase 这些后面都用netty 来做节点间数据的传输（参见TaskManager之间的shuffle）

从Akka与Actor模型入手，以点带面，一步步引出Flink的整体组件通信全景。对Flink中RPC框架涉及的主要类RpcGateway、RpcEndpoint、RpcService、RpcServer、AkkaRpcActor进行仔细拆解，之后通过代码的跳转详细分析了RPC的交互过程。


# RpcGateway接口
定义了RPC通讯的协议
只有两个方法：getAddress() 和getHostName()


# RpcEndpoint implements RpcGateway
RpcEndpoint含义：通讯的端，即Actor。
类注释：
* 提供远程过程调用的分布式组件都需要继承基础类RpcEndpoint。
* Single Threaded Endpoint Execution：在同一个endpoint上的所有RPC调用都被主线程（main thread）完成。AKKA的单线程？？TODO
* lifecycle：Created，start()/onstart()，执行请求，close/onStop

内部类：MainThreadExecutor，Executor which executes runnables in the main thread context。
提供：runAsync、scheduleRunAsync、execute、schedule、scheduleAtFixedRate、scheduleWithFixedDelay等方法。


# FencedRpcEndpoint
![image](https://user-images.githubusercontent.com/42859030/110249180-e00ec200-7faf-11eb-8db5-787fb2b68cfe.png)


## 原理背景：
Fencing token:一种资源保护机制。（fenced：围起来的 -> 即隔离机制。）
机制原理：
https://www.jianshu.com/p/73512e7730fd
![image](https://user-images.githubusercontent.com/42859030/110233371-8205be80-7f5e-11eb-82b9-2ec35e7961df.png)

参见：Hadoop的fencing机制
https://www.cnblogs.com/lushilin/p/11239908.html

## FencedRpcEndpoint原理
给RpcEndpint增加了一个fencing-token，这是一个连续增长的整数(fencing-token的本意)，FencedRpcEndpoint只处理那些**附带的fencing-token跟自己的fencing-token一样的消息**，这个功能明确地指定**让某个actor处理某个消息**。
FencedRpcEndpoint中包含一个重要的属性：fencedMainThreadExecutor，它是一个MainThreadExecutor（主线程执行器），其中有一个属性**MainThreadExecutable**：
### MainThreadExecutable定义 
在主线程中，执行RPC endpoint的Runnable和Callable：
    void runAsync(Runnable runnable);
    <V> CompletableFuture<V> callAsync(Callable<V> callable, Time callTimeout);
    void scheduleRunAsync(Runnable runnable, long delay);

### FencedMainThreadExecutable extends MainThreadExecutable
除了MainThreadExecutable的几个方法，还有两个自己的方法：
    void runAsyncWithoutFencing(Runnable runnable); 
    <V> CompletableFuture<V> callAsyncWithoutFencing(Callable<V> callable, Time timeout);






# Java 知识
## 成员变量和方法的访问修饰符：
private：私有，只有本类可以访问
default：本类+同一个包
protected：本类、同一个包+不同包的子类可以访问
public：本类、同一个包、不同包的非子类类均可以访问
