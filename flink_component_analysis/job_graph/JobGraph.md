## 概览
在执行Flink任务的时候，会涉及到三个Graph，分别是StreamGraph，JobGraph，ExecutionGraph。
其中StreamGraph和JobGraph是在client端生成的，ExecutionGraph是在JobMaster中执行的。
* StreamGraph是根据用户代码生成的最原始执行图，也就是直接翻译用户逻辑得到的图
* JobGraph是对StreamGraph进行优化，比如设置哪些算子可以chain，减少网络开销
* ExecutionGraph是用于作业调度的执行图，对JobGraph加了并行度的概念



## JobGraph:
JobGraph是由顶点（JobVertex）、作业边（JobEdge）以及中间结果集（IntermediateDataSet）三个基本的元素组成。其中JobEdge和IntermediateDateSet都是多个Vertex之间的连接的纽带，分别代表顶点的input和result。


## ExecutionGraph
## DAG
