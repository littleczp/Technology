# HTTP3

<details>

<summary>QUIC使用UDP的主要原因?</summary>

它能让 HTTP/3 更容易部署，因为 UDP 已经被互联网上几乎所有的设备所知并实现

* 低延迟：无连接（0-RTT或1-RTT握手）
* 多路复用：单个UDP连接上可以同时处理多个数据流
* 丢包恢复：QUIC通过在应用层实现可靠性机制来处理丢包和重传
* 防止队头阻塞：TCP在传输过程中存在队头阻塞问题

</details>

***

HTTP/1.1 和 HTTP/2 都存在队头阻塞问题（Head of line blocking）

{% tabs %}
{% tab title="HTTP/1.1" %}
HTTP/1.1一个 TCP 连接同时传输 10 个请求，其中第 1、2、3 个请求已被客户端接收，但第 4 个请求丢失，那么后面第 5 - 10 个请求都被阻塞，

需要等第 4 个请求处理完毕才能被处理，这样就浪费了带宽资源
{% endtab %}

{% tab title="HTTP/2" %}
HTTP/2 中每个请求都被拆分成多个 Frame 通过一条 TCP 连接同时被传输，

不同请求的 Frame 组合成 Stream，这样即使一个请求被阻塞，也不会影响其他的请求

\
HTTP/2 虽然可以解决 "请求" 这个粒度的阻塞，但 HTTP/2 的基础 TCP 协议本身却也存在着队头阻塞的问题：

在一条 TCP 连接上同时发送 4 个 Stream，其中 Stream1 已正确送达，Stream2 中的第 2 个 Frame 丢失，TCP 处理数据时有严格的前后顺序，先发送的 Frame（帧） 要先被处理，这样就会要求发送方重新发送第 2 个 Frame，Stream3 和 Stream4 虽然已到达但却不能被处理，那么这时整条连接都被阻塞
{% endtab %}
{% endtabs %}









