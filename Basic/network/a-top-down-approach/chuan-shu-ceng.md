# 传输层

<details>

<summary>流量控制与拥塞控制</summary>

<table><thead><tr><th width="117.90252685546875"></th><th width="173.7166748046875">目的</th><th>desc</th></tr></thead><tbody><tr><td>流量控制</td><td>端到端的控制</td><td><p>A通过网络给B发数据（对端），A发送的太快导致B没法接收(B缓冲窗口过小或者处理过慢)，导致分组丢失</p><p></p><p>原理是通过滑动窗口的大小改变来实现（防止分组丢失）</p></td></tr><tr><td>拥塞控制</td><td>防止路由器接收不住</td><td><p>A与B之间的网络发生堵塞导致传输过慢或者丢包，来不及传输。</p><p></p><p>防止过多的数据注入到网络中（全局），这样可以使网络中的路由器或链路不至于过载</p></td></tr></tbody></table>

</details>



高并发短连接的TCP服务器上，当服务器处理完请求后立刻主动关闭连接。

这个场景下会出现大量socket处于TIME\_WAIT状态

<details>

<summary>大量socket处于TIME_WAIT状态</summary>

* 编辑内核文件/etc/sysctl.conf，修改TCP相关配置
* 将短连接变成长链接
* 修改net.ipv4.ip\_local\_port\_range限制
* 设置time\_wait时间

</details>
