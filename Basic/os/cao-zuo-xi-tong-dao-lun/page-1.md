# Page 1

程序运行时会发生什么?

从内存中获取一条指令（fetch） => 解码（decode） => 执行 (execute） => loop



{% tabs %}
{% tab title="虚拟化cpu" %}
在硬件的帮助下，将单个CPU转换为看似无限数量的CPU，从而让许多程序看似同时运行
{% endtab %}

{% tab title="虚拟化内存" %}
使得每个正在运行的程序都有自己的私有内存 -> 以某种方式映射道机器的物理内存上
{% endtab %}
{% endtabs %}



