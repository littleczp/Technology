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

#### 缓冲池

通过内存的速度来弥补磁盘速度较慢的影响

<table data-header-hidden><thead><tr><th width="116.6666259765625">opt</th><th></th></tr></thead><tbody><tr><td>读取页</td><td><p>将磁盘数据页（包括聚集索引和非聚集索引）从磁盘读到的页存放在缓冲池中</p><p></p><p>再次读取相同页时，先判断该页是否在缓冲池中，如果在则命中，否则读磁盘</p></td></tr><tr><td>修改页</td><td>先修改在缓冲池中的页，再以一定的频率刷新到磁盘上（Checkpoint）</td></tr></tbody></table>

<details>

<summary>如何对缓冲池进行管理</summary>

LRU，midpoint 位置（<mark style="color:red;">5/8</mark>）处：直接将读取到的页放到LRU首部的话，那么某些SQL操作可能会使缓冲池中的页被刷新出，从而影响缓冲池的效率。比如一次扫描操作

</details>

#### Insert Buffer（不属于缓冲池）

只有辅助索引需要使用插入缓冲，同时整个辅助索引不能包含唯一字段（离散读取）

{% tabs %}
{% tab title="Primary Key" %}
```sql
CREATE TABLE t (
    a INT AUTO_INCREMENT,
    b VARCHAR(30),
    PRIMARY KEY(a)
)
```

如果没有设置主键，则会找到其中一个唯一的列构建（如果没有就用隐藏列）

页中的记录按照a的值进行顺序存放：**顺序插入（逻辑上连续），不需要磁盘的随机读取**
{% endtab %}

{% tab title="Secondary Index" %}
```sql
CREATE TABLE t (
    a INT AUTO_INCREMENT,
    b VARCHAR(30),
    PRIMARY KEY(a),
    key(b)
)
```

在进行插入操作时，数据页的存放还是按照主键a进行顺序存放

但对于非聚集索引叶子节点的插入不是顺序，需要**离散地访问非聚集索引页**
{% endtab %}
{% endtabs %}

#### Double Write

写入磁盘的过程中，保证数据页的可靠性

* Doublewrite Buffer，大小为2MB
* 物理磁盘上共享表区间连续的128个页（2个区），大小也为2MB

对缓冲池的脏页进行刷新时，并不直接写磁盘，而是通过memcpy（memory copy）函数将脏页先复制到内存中的Double Write Buffer。接着通过Double Write Buffer再分两次，每次1MB顺序地写入共享表空间的物理磁盘上

{% hint style="info" %}
如果在写入磁盘过程中发生了崩溃，在恢复过程中InnoDB可以从共享表空间中的Double Write中找到该页的一个副本，将其复制到表空间文件，再应用重做日志
{% endhint %}

<details>

<summary>为什么不直接使用Redo Log来恢复</summary>

Redo Log是记录对页的物理操作，如果页本身已经损坏，在进行重做没有意义

所以在应用重做日志前，用户需要一个页的副本，在发生写入失效的时候，先通过页的副本还原该页，再进行重做

</details>

