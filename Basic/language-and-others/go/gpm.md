# GMP

G-P-M 模型，它包括 4 个重要结构，分别是G、P、M、Sched

{% tabs %}
{% tab title="Goroutine" %}
**存储运行堆栈、状态以及任务函数**，每个 Goroutine 对应一个 G 结构体，可重用。



G 并非执行体，每个 G 需要绑定到 P 才能被调度执行
{% endtab %}

{% tab title="Processor" %}
提供了相关的执行环境(Context)，如内存分配状态(mcache)，任务队列(G)等

"并行"逻辑处理器，P 的数量由用户设置的 GoMAXPROCS 决定，P 的数量最大为 256（物理 CPU 核数  >= P 的数量）



每个 P 都被分配到一个系统线程 M
{% endtab %}

{% tab title="Machine" %}
OS 内核线程抽象，代表着真正执行计算的资源



在绑定有效的 P 后，进入 schedule 循环: 从 Global 队列、P 的 Local 队列以及 wait 队列中获取

M 的数量是不定的，由 Go Runtime 调整，为了防止创建过多 OS 线程导致系统调度不过来，目前默认最大限制为 10000 个
{% endtab %}

{% tab title="Sched" %}
Go 调度器，它维护有存储 M 和 G 的队列以及调度器的一些状态信息等
{% endtab %}
{% endtabs %}





