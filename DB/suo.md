# 锁

<table><thead><tr><th width="105.6666259765625">特性</th><th width="338">共享锁</th><th>排他锁</th></tr></thead><tbody><tr><td>兼容性</td><td>允许多个事务<strong>同时持有</strong>对同一数据</td><td>同一时间<strong>只有一个事务</strong>能持有对某数据</td></tr><tr><td><strong>阻塞场景</strong></td><td>仅阻塞X锁请求</td><td>阻塞所有S锁和X锁请求</td></tr><tr><td>操作</td><td><code>SELECT ... LOCK IN SHARE MODE</code></td><td><code>UPDATE/DELETE/SELECT ... FOR UPDATE</code></td></tr><tr><td>实现原理</td><td><ul><li>InnoDB在行记录的头信息中标记锁状态</li></ul><ul><li>多个S锁通过引用计数管理（无需排队）</li></ul></td><td><ul><li>修改数据时，先获取X锁并写入undo log</li></ul><ul><li>其他事务访问时会触发锁等待</li></ul></td></tr></tbody></table>

<details>

<summary>锁的实现原理</summary>

* 锁本质上是一个**等待队列**，记录事务ID和锁模式
* 通过位图（bitmap）管理行锁，存储在**事务系统内存区**

</details>

<details>

<summary><em>为什么MVCC不能完全替代锁？</em></summary>



</details>

