# DB

<details>

<summary>MVCC</summary>



</details>

<details>

<summary>可重复读（<strong>Repeatable Read</strong>）和幻读<strong>（Phantom Read）</strong>？</summary>

可重复读：事务内**多次读取同一数据**的结果一致（即使其他事务已修改并提交）

* **MVCC机制**：事务首次读时生成**ReadView**，后续读沿用该视图。但快照读无法阻止其他事务插入新数据，所以不能解决幻读

幻读：事务内**两次相同条件查询**，结果集行数不同（侧重<mark style="color:red;">结果集</mark>变化）

* 使用临键锁：行锁+间隙锁（左开右闭）+ MVCC解决幻读
* 查询为for update，需要有索引，否则间隙锁会失效，降级为表锁；如果索引有唯一属性，InnoDB会优化为行锁

</details>

<details>

<summary>事务ACID</summary>

<table><thead><tr><th width="119.33331298828125">ACID</th><th>Desc</th><th>实现</th></tr></thead><tbody><tr><td>Ａ，原子性</td><td>事务全做或者全不做</td><td>Undo Log，记录回滚信息</td></tr><tr><td>C，一致性</td><td>在事务开始前和事务结束后，数据库的完整性约束没有被破坏（回滚后回到初始化的状态）</td><td>主键/外键约束、触发器、应用层校验</td></tr><tr><td>I，隔离性</td><td>并发事务互不干扰</td><td>临键锁 + MVCC</td></tr><tr><td>D，持久性</td><td>提交后数据永久保存</td><td>Buffer Pool + Redo Log</td></tr></tbody></table>

</details>

<details>

<summary>分布式事务</summary>

1. 两段式 / 三段式提交

</details>

<details>

<summary>为什么推荐自增主键 / 为什么用整形（BIGINT）不用UUID？</summary>

1. 无序数据必须分裂页
   1. 分裂时阻塞写入（锁页）：分配新页 -> 移动部分数据 -> 更新父节点指针
   2. 增加碎片：页利用率下降，查询性能下降
   3. 随机I/O增多
2. 自增主键
   1. 顺序写入：新数据总是追加到B+树最右侧，避免分裂（级联分裂）
   2. B+数高度更低，范围查询更快
   3. 缓存友好：连续主键提升缓冲池命中率
   4. 空间紧凑：页填充率通常更高（页填充率15/16，当页剩余空间<1/16时触发分裂机制）

UUID占用空间更多（16字节），BigINT（8字节）

</details>

<details>

<summary>主从结构</summary>



</details>

