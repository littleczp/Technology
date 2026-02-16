# 应用层

<details>

<summary>HTTP各版本的差别</summary>

<table><thead><tr><th width="116.47088623046875">版本</th><th>特点</th></tr></thead><tbody><tr><td>http0.9</td><td>仅支持请求方式GET</td></tr><tr><td>http1.0</td><td><ul><li>增加请求方式：POST、HEAD</li><li>支持多种数据格式（Content-Type）</li></ul></td></tr><tr><td>http1.1</td><td><ul><li>新增了请求方式：PUT、PATCH、OPTIONS、DELETE</li><li>引入了持久连接：在同一个TCP连接里，允许多个请求同时发送</li></ul></td></tr><tr><td>http2.0</td><td><ul><li>引入了多路复用的技术：解决浏览器限制同一个域名下的请求数量</li><li>彻底的二进制协议</li></ul></td></tr><tr><td>http3.0</td><td><ul><li>传输层 0RTT 就能建立连接</li><li>加密层 0RTT 就能建立加密连接</li></ul></td></tr></tbody></table>

</details>

***

<details>

<summary>HTTP/1.x的缺陷</summary>

```
连接无法复用：导致每次请求都经历三次握手和慢启动
```

* HTTP/1.0：传输数据时，每次都需要重新建立连接，增加延迟
* HTTP/1.1：虽然加入 keep-alive 可以复用一部分连接，但域名分片等情况下仍然需要建立多个 connection，耗费资源，给服务器带来性能压力

</details>

<details>

<summary>HTTP/2.x的缺陷</summary>

HTTP/2 使用了多路复用，一般来说同一域名下只需要使用一个 TCP 连接。但在出现丢包的情况下，整个 TCP 都要开始等待重传，也就导致了后面的所有数据都被阻塞了

</details>

***

<details>

<summary>DNS</summary>

Domain name system runs over UDP and uses port 53

| root DNS servers                   | provide the IP addresses of the TLD servers            |                                                                |
| ---------------------------------- | ------------------------------------------------------ | -------------------------------------------------------------- |
| top-level domain (TLD) DNS servers | provide the IP addresses for authoritative DNS servers | <p>com, org, net, edu, and gov</p><p>uk, fr, ca, cn and jp</p> |
| authoritative DNS servers          | Every organization with publicly accessible hosts      | Web servers(google)                                            |

</details>
