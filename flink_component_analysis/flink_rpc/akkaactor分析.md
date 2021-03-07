# AkkaRpcActor分析


## 处理RPC消息
```java
protected void handleRpcMessage(Object message) {
    if (message instanceof RunAsync) {
        handleRunAsync((RunAsync) message);
    } else if (message instanceof CallAsync) {
        handleCallAsync((CallAsync) message);
    } else if (message instanceof RpcInvocation) {
        handleRpcInvocation((RpcInvocation) message);
    }
}
```
第一种：
```java
private void handleRunAsync(RunAsync runAsync){
        runAsync.getRunnable().run();
}
```
第三种：
handleRpcInvocation(RpcInvocation rpcInvocation):
通过在RPC endpoint上寻找RPC方法并用提供的参数执行该方法，来处理RPC请求。
Handle rpc invocations by looking up the rpc method on the rpc endpoint and calling this
method with the provided method arguments. If the method has a return value, it is returned
to the sender of the call.
### RpcInvocation
#### RemoteRpcInvocation 远程调用
* 有序列化操作
内部类，MethodInvocation

#### LocalRpcInvocation 本地调用
* 没有序列化操作
