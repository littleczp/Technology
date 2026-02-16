# 应用层

<details>

<summary>HTTP各版本的差别</summary>

<table><thead><tr><th width="116.47088623046875">版本</th><th>特点</th></tr></thead><tbody><tr><td>http0.9</td><td>仅支持请求方式GET</td></tr><tr><td>http1.0</td><td><ul><li>增加请求方式：POST、HEAD</li><li>支持多种数据格式（Content-Type）</li></ul></td></tr><tr><td>http1.1</td><td><ul><li>新增了请求方式：PUT、PATCH、OPTIONS、DELETE</li><li>引入了持久连接：在同一个TCP连接里，允许多个请求同时发送</li></ul></td></tr><tr><td>http2.0</td><td><ul><li>引入了多路复用的技术：解决浏览器限制同一个域名下的请求数量</li><li>彻底的二进制协议</li></ul></td></tr><tr><td>http3.0</td><td><ul><li>传输层 0RTT 就能建立连接</li><li>加密层 0RTT 就能建立加密连接</li></ul></td></tr></tbody></table>

</details>
