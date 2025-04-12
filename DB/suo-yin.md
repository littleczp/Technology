# 索引

## 覆盖索引聚集索引

按照主键构建一棵B+树，行记录数据放在叶子节点（16KB数据页）中

{% hint style="info" %}
在多数情况下，查询优化器倾向于采用聚集索引

* 能够在B+索引的叶子节点上直接找到数据
* 能够快速访问针对范围值的查询
{% endhint %}

## 非聚集索引

辅助索引，叶子节点存储的是聚集索引主键的key

<details>

<summary>为什么不放入数据而放入主键指针</summary>

1. 占用更多的磁盘空间
2. 数据更新时需要维护对所有索引树的更新

</details>

<table><thead><tr><th width="159.66668701171875"></th><th width="234.666748046875">聚集索引</th><th>非聚集索引</th></tr></thead><tbody><tr><td>主键查询/物理存储</td><td>直接定位数据页（O(1) I/O）</td><td>需要二次回表查询（主键→数据页）</td></tr><tr><td>范围查询</td><td>顺序读取磁盘，效率高</td><td>离散读取，需要多次回表（多次I/O）</td></tr><tr><td></td><td></td><td>包含查询所需字段时可避免回表</td></tr><tr><td>插入性能</td><td>可能导致页分裂</td><td>仅更新索引树，开销较小</td></tr></tbody></table>

<details>

<summary>页剩余空间小于<strong>1/16</strong>（即15/16已满）时触发分裂，而不是1/32？</summary>

* 空间利用率仅增加约3%
* 但分裂频率上升40%（严重影响高并发写入）

</details>

## 联合索引

## 覆盖索引

<details>

<summary>count(1)、count(*) 与 count(column)</summary>

<table><thead><tr><th width="149.33331298828125">covering index</th><th>effect</th><th>null</th></tr></thead><tbody><tr><td>count(*)</td><td>忽略列，用1代表行</td><td>包含NULL值</td></tr><tr><td>count(1)</td><td>所有列(行数)</td><td>包含NULL值</td></tr><tr><td>count(column)</td><td>只包括列名那一列</td><td>忽略NULL</td></tr></tbody></table>

COUNT(\*) 通常是最快的方式，因为它不需要读取任何列的数据，只需计算行数

</details>

## 优化器选择不使用索引的情况

{% tabs %}
{% tab title="select *" %}
辅助索引不能覆盖要查询的信息，访问量数据>20%，会选择聚集索引
{% endtab %}

{% tab title="类型不匹配" %}
| 0   | 0   | 0   |
| --- | --- | --- |
| 1   | 1   | 1   |
| ... | ... | ... |

```sql
select * from x where a = "a";
```
{% endtab %}

{% tab title="对字段做了操作" %}
```sql
select *
from xxx
where a + 1 =  2;
```
{% endtab %}

{% tab title="排序后不符合最左前缀顺序" %}
order by desc b, c（5.8前都是升序索引）
{% endtab %}
{% endtabs %}

