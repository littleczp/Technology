# 服务治理

<details>

<summary>如何进行服务发现</summary>

启动一个单独的进程称为：Service Mesh，它是一个轻量级的网络代理程序，通过 side car 模式部署在应用程序旁边，实现统一高效的服务发现、服务治理、安全审计等功能

* 语言碎片化：字节内存在多种语言
* 协议碎片化：Thrift 不同版本之间互不兼容；线上 HTTP、Thrift、gRPC 等协议并存；proxy 定义了一个 TTHeader 的标准，只需要满足标准就可以解决流量管理（超时配置、跨机房调度）
* 实现业务自定义分流：ACL（访问控制）、PPE（预生产环境）、BOE（泳道环境）
* 通过选定的did(device-id)向对应流量注入标识，control plane会下发分流指令，把流量打到指定的下游服务
* 通过熔断、限流等手段可以增加弹性和容错，同时也可以通过策略实现负载均衡

</details>



<details>

<summary>Service Mesh的实现</summary>

Mesh 有三个组成部分

* 数据面：proxy
* 控制面：control plane
* 运维面

&#x20;

1. 在每个容器创建时，mesh agent 会先于其他进程启动，然后负责拉起 mesh proxy 和 业务进程并作为单独进程持续存在。它监控 mesh proxy 和业务进程的生命周期，同时还承担着 mesh proxy 的版本控制、热更新等功能
2. proxy 运行后，control plane 将运维面的 serivce mesh 配置和策略下发到各个 proxy 中
3. 应用程序完成初始化，proxy 开始与 control plane 通信，上报服务信息并完成应用程序注册
4. proxy 开始拦截应用程序的网络流量，并根据配置进行流量路由、负载均衡和策略应用

</details>



<details>

<summary>LB如何发现服务不可用，比如某域名挂掉？挂掉后如何切换</summary>

通过 LB 的健康检查机制（更新路由表）

* TCP 检查：建立 TCP 链接
* HTTP 检查：发送 HTTP 请求到后端服务器的特定端口，通过响应状态码来判断服务器是否正常提供服务
* 自定义脚本：进行更复杂的健康检查，检查特定的服务状态 / 返回特定数据



通过 LB 的故障转移机制

* 从服务池中移除故障服务
* Keepalived与LVS的结合使用，使得主服务器失效时可以由备用服务器自动接管；或者通过一些favour设置能够实现切换

</details>



