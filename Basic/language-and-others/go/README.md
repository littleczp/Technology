# Go

<details>

<summary>Python与Go的对比</summary>

不简单判断谁更好：Python 的优势是开发效率高、生态丰富；Go 的优势是编译型、静态类型

如果是高并发 API、网关、长连接服务、内部 RPC、中间件，我更倾向 Go；

如果是数据处理、AI 服务编排、运营后台、快速原型、脚本任务，我更倾向 Python。



实际项目里也经常会组合使用：Python 负责算法、数据处理，Go 负责高并发接入层、核心服务和基础设施组件。

</details>



<details>

<summary>GO为什么在高并发场景性能更佳</summary>

因为 goroutine 足够轻量（2KB），runtime 调度器能把大量 goroutine 复用到少量 OS 线程上；网络 IO 底层结合 epoll/kqueue 等多路复用机制，让同步写法具备高并发能力

</details>

