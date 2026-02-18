# 死锁

{% tabs %}
{% tab title="互斥" %}
资源独占
{% endtab %}

{% tab title="占有等待" %}
进程持有了锁，同时又在等待其他锁
{% endtab %}

{% tab title="不可剥夺" %}
由持有它的进程主动释放，**不能被强行抢走**
{% endtab %}

{% tab title="循环等待" %}
存在一个环路，每个进程都额外持有一个锁，而这个锁是下一个进程需要申请的
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="互斥" %}
无锁结构，设计wait-free数据结构
{% endtab %}

{% tab title="占有等待" %}
通过原子地抢占锁来避免死锁，先获取全局锁
{% endtab %}

{% tab title="不可剥夺" %}
使用trylock()尝试获得锁，如果返回-1则表示锁被占有

（但可能会导致活锁，即两个线程不断争取锁但一直失败）
{% endtab %}

{% tab title="循环等待" %}
在获取锁时，提供一个 total ordering 或 partial ordering(Linux偏序锁)
{% endtab %}
{% endtabs %}
