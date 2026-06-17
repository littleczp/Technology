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



<details>

<summary>GO中goroutine调度</summary>

Go runtime 通过 **GMP 调度模型**，把大量 goroutine 调度到少量操作系统线程上执行，从而以较低成本支撑高并发。



G 表示 goroutine，M 是系统线程，P 是调度器上下文。M 必须绑定 P 才能执行 G。每个 P 有本地队列，调度器优先从本地队列取 G，没任务时从全局队列或其他 P 获取任务。网络 IO 会交给 netpoller，系统调用阻塞时 P 会和 M 分离，从而避免一个阻塞线程拖住整个调度器。这个模型让 Go 能用较少线程支撑大量 goroutine，并且较好利用多核 CPU。

</details>
