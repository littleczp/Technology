# MySQL体系结构和存储引擎

存储引擎是基于表的，不是基于数据库的



## MyISAM

不支持事务、表锁设计

{% hint style="info" %}
在5.5.8前，MyISAM是默认存储引擎
{% endhint %}

## InnoDB

### 后台线程

{% tabs %}
{% tab title="Master " %}
将缓冲池中的数据异步刷新到磁盘，保证数据的一致性



包括：脏页的刷新、合并插入缓冲、UNDO页的回收等
{% endtab %}

{% tab title="IO" %}
负责写IO请求的call back处理
{% endtab %}

{% tab title="Purge" %}
回收已经使用并分配的undo页
{% endtab %}

{% tab title="Page Cleaner" %}
刷新脏页，减轻Master Thread的工作
{% endtab %}
{% endtabs %}

### 内存

通过内存的速度来弥补磁盘速度较慢的影响

<table data-header-hidden><thead><tr><th width="116.6666259765625"></th><th></th></tr></thead><tbody><tr><td>读取页</td><td><p>将从磁盘读到的页存放在缓冲池中（"FIX"）</p><p></p><p>再次读取相同的页时，先判断该页是否在缓冲池中，如果在则命中，否则读磁盘</p></td></tr><tr><td>修改页</td><td>先修改在缓冲池中的页，再以一定的频率刷新到磁盘上（Checkpoint）</td></tr></tbody></table>
