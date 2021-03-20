## Actor模型

Akka系统的核心ActorSystem和Actor。
若需构建一个Akka系统，首先需要创建ActorSystem，创建完ActorSystem后，可通过其创建Actor
（注意：Akka不允许直接new一个Actor，只能通过 Akka 提供的某些 API 才能创建或查找 Actor，一般会通过 ActorSystem#actorOf和ActorContext#actorOf来创建 Actor）。
另外，我们只能通过ActorRef（Actor的引用， 其对原生的 Actor 实例做了良好的封装，外界不能随意修改其内部状态）来与Actor进行通信。

### Actor
```json
The previous section about Actor Systems explained how actors form hierarchies and are the smallest unit when building an application.
```
actor是构建应用的最小单元。
Actor可以创建子Actor。

### ActorSystem：Actor系统
* 管理调度服务:将任务分拆成最小的可执行单元
* 配置相关参数：根据配置环境完成整个系统的环境配置
* 日志功能
```json
An ActorSystem is a heavyweight structure that will allocate 1…N Threads, so create one per logical application.
```
ActorSystem是一个分配多个线程重量级的结构。所以应该给每个逻辑应用创建一个ActorSystem。

### ActorRef：Actor引用，Actor的代理

### Actor path和地址
树形目录来记录地址

### rpcService
todo

## Akka重要函数
tell:异步发送一个消息并立即返回。
ask:异步发送一条消息并返回一个 Future代表一个可能的回应


参考
https://blog.csdn.net/u013411339/article/details/95041930?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-0&spm=1001.2101.3001.4242 你有必要了解一下Flink底层RPC使用的框架和原理
