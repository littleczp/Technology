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



G 表示 goroutine，M 是系统线程（Machine），P 是调度器上下文（Processor）。M 只有关联了 P，才能执行 Go 代码。每个 P 有本地队列，调度器优先从本地队列取 G，没任务时从全局队列或其他 P 获取任务。

网络 IO 会交给 netpoller，系统调用阻塞时 P 会和 M 分离，从而避免一个阻塞线程拖住整个调度器。

</details>



<details>

<summary>GO的网络模型</summary>

Go的网络模型主要基于 I/O Multiplexing 和 Goroutine Scheduler（非阻塞IO）

I/O Multiplexing：不同系统下实现有所不同(select、epoll、kqueue)，在Linux系统下基于epoll

<table data-header-hidden><thead><tr><th>I/O 多路复用</th><th>desc</th><th valign="middle">compare</th></tr></thead><tbody><tr><td>select</td><td>监视一组文件描述符，等待其中的一个或多个变为可读、可写或有异常</td><td valign="middle"><p></p><ol><li>开销大，需要从将用户态的文件描述符拷贝到内核态</li><li>每次循环都需要清空描述符集合，所以每一次循环都需要解决所有就绪的IO事件</li><li>select需要固定数组的大小来存储监视的文件描述符（Linux 1024，bit array），所以也限制了最大连接数</li></ol></td></tr><tr><td>epoll</td><td><p>允许程序注册感兴趣的事件，当这些事件发生时，epoll会通知程序<br> </p><p>通过epoll create创建epoll实例，通过epoll ctrol将文件描述符添加到epoll实例中。使用epoll wait获取所有就绪的文件描述符</p></td><td valign="middle"><p>显著提高程序在高并发连接中，只有少量是活跃的情况下的系统CPU利用率</p><p>它无须遍历整个被侦听的描述符集（poll需要遍历整个链表/数组），只要遍历那些被内核IO事件异步唤醒而加入Ready队列的描述符集合</p></td></tr></tbody></table>

</details>
