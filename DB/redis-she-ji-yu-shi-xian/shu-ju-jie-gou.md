# 数据结构

<details>

<summary><strong>Redis 数据结构</strong></summary>

<table><thead><tr><th width="133.75592041015625">数据结构</th><th>实现</th><th>场景</th></tr></thead><tbody><tr><td>String</td><td>SDS</td><td><p>计数器（INCR）</p><p>分布式锁（SETNX）</p></td></tr><tr><td>List</td><td>双向链表/压缩列表(ziplist)</td><td>消息队列、最新消息排行</td></tr><tr><td><strong>Hash</strong></td><td>字典+ziplist</td><td>对象属性存储</td></tr><tr><td>Set</td><td>字典+ziplist</td><td>标签系统、共同好友</td></tr><tr><td><strong>Sorted Set</strong></td><td>跳表+字典</td><td>排行榜、延迟队列</td></tr><tr><td><strong>Bitmap</strong></td><td>String扩展</td><td>用户签到、实时统计</td></tr><tr><td><strong>HyperLogLog</strong></td><td>专用基数算法</td><td>UV统计</td></tr><tr><td><strong>Stream</strong></td><td>紧凑列表+消费组</td><td>消息队列（类Kafka）</td></tr><tr><td>GEO</td><td>Sorted Set扩展</td><td>地理位置计算</td></tr></tbody></table>



</details>

