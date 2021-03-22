# checkpoint 
检查点是Flink为流计算过程提供的容错和故障恢复机制。
Flink作业可以抽象成有向图表示，图的顶点是算子（operator），边是数据流（data stream）

# checkpoint 原理
## 通过状态快照实现容错处理
https://ci.apache.org/projects/flink/flink-docs-release-1.12/zh/learn-flink/fault_tolerance.html
### State Backends
Flink 管理的状态存储在 state backend 中。

### 状态快照
Checkpoint – 一种由 Flink 自动执行的快照，其目的是能够从故障中恢复。Checkpoints 可以是增量的，并为快速恢复进行了优化。

## 原理：异步屏障快照
ABS（asynchronous barrier snapshotting)
https://www.jianshu.com/p/3093f6d92750
![image](https://user-images.githubusercontent.com/42859030/112030091-dc189d80-8b74-11eb-90ca-e333fd31f58f.png)

第n - 1个屏障之后、第n个屏障之前的所有数据都属于第n个检查点。
下游算子如果检测到屏障的存在，就会触发快照动作，不必再关心时间无法静止的问题。
