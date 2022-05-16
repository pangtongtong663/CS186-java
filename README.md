# CS186-java
My implement
## Background
  该项目属于UC Berkeley大学的CS186课程。该项目用Java实现一个支持SQL并发查询、B+树索引、join构建、查询优化，以及Aries故障恢复的关系型数据库。
## Structure
下面列举出该项目主要实现内容所在的包以及类
* index：index包中包含了数据库索引中B+树的实现
    * ```LeafNode.java```: B+树索引的叶子节点实现
    * ```InnerNode.java```: B+树索引的中间结点实现
    * ```BPlusTree.java```: B+树索引的整体实现
* query: query包中包含了数据库各种join算法实现，以及Query Optimization的实现
    * ```BNLJOperator.java```: Block Nested Loop Join的算法实现
    * ```GHJOperator.java```: Grace Hash Join的算法实现
    * ```SortMergeOperator.java```: Sort Merge Join的算法实现
    * ```SortOperator.java```: External Sort的算法实现
    * ```QueryPlan.java```：采用动态规划的思想，从单表查询优化选择(Pass i = 1)，到多表的join选择(Pass i > 1)，进而实现总体的优化查询方案。其中Join order采用左深树的方式。
* concurrency: concurrency包中包含了锁类型、锁管理、多粒度锁、锁升级，二阶段加锁等内容。
    * ```LockType.java```：包含了```S```、```IS```、```X```、```IX```和```SIX```的锁类型。以及一些锁是否兼容，parentLock是否兼容等判断方法。
    * ```LockManager.java```: 实现了锁管理的最底层。包含锁的获取、释放，升级等底层操作。
    * ```LockContext.java```：实现了锁管理的中间层。侧重于多粒度锁的操作实现。
    * ```LockUtil.java```：实现了锁管理的最顶层。协调调用中间层和最底层锁管理方法，从而实现所需锁功能。
    * 注意：两阶段加锁主要体现在项目整体，如Page.PageBuffer的get和put方法等。
* recovery：recovery包主要采用Aries算法实现数据库的故障恢复。
   *  ```ARIESRecoveryManager```：包含forward process的savepoints、checkpoins、transaction status、log功能，以及故障恢复时的Analysis阶段、Redo阶段和Undo阶段的实现。
