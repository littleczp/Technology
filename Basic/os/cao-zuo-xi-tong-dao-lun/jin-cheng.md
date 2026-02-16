# 进程

<details>

<summary>并发与并行</summary>

|                 | 依赖                 | Desc                                     |
| --------------- | ------------------ | ---------------------------------------- |
| 并发(Concurrency) | 时分共享（Time Sharing） | 通过让一个进程只运行一个时间片，然后切换到其他进程，从而实现用户运行多个并发进程 |
| 并行(Parallelism) | 多核CPU              | 依赖物理硬件资源，使得多个任务在同一时刻同时运行                 |

</details>



<details>

<summary>堆和栈</summary>

<table><thead><tr><th width="82.03375244140625"></th><th>存储内容</th><th>分配和释放</th><th>访问时间</th></tr></thead><tbody><tr><td>堆</td><td>动态内存分配、数据结构、类的实例</td><td>调用函数（malloc &#x26; free），分配时需要到堆空间去寻找足够大小的空间</td><td>需要两次访问内存，第一次得取得指针，第二次才是真正得数据</td></tr><tr><td>栈</td><td>局部变量、函数参数、返回地址</td><td>系统自动分配</td><td>只需访问一次</td></tr></tbody></table>

</details>



<details>

<summary>进程、线程、协程</summary>

<table><thead><tr><th width="84.954833984375"></th><th>概念</th><th>作用</th><th>组成</th></tr></thead><tbody><tr><td>进程</td><td>操作系统为正在执行的程序提供的抽象，是资源分配的最小单位（2GB）</td><td><ol><li>将代码、静态数据从磁盘加载到内存（进程的空间地址）中</li><li>为栈分配内存</li><li>为堆分配内存</li><li>完成其他初始化任务：I/O（标准输入、输出、错误）</li></ol></td><td><p></p><ol><li>PCB（进程控制块）</li><li>程序段</li><li>数据段</li></ol></td></tr><tr><td>线程</td><td>CPU任务调度（程序执行）的最小单位，一个进程中可以包含多个线程来完成多个任务</td><td>任务之间共享进程提供的代码和数据空间</td><td><p></p><ol><li>独立的运行栈</li><li>程序计数器</li></ol></td></tr><tr><td>协程</td><td><p>用户态的轻量级线程，一个线程可以有多个协程</p><p>由主线程创建事件循环，依次执行事件循环中的事件，通过操作系统原语(yield)主动交出控制权进行上下文切换并执行下一个事件</p></td><td><p>协程的调度完全由用户控制（也就是在用户态执行），</p><p>协程调度切换时，会将寄存器上下文和栈保存到线程的堆区，在切回来的时候，恢复先前保存的寄存器上下文和栈</p></td><td><p></p><ol><li><p>寄存器上下文</p><ol><li>程序计数器（PC）</li><li>通用寄存器：正在处理的数据</li><li>特殊寄存器：硬件状态及配置</li></ol></li><li>栈</li></ol></td></tr></tbody></table>

</details>



<details>

<summary>多进程和多线程如何选择</summary>



<table><thead><tr><th width="79.3182373046875"></th><th width="246.82183837890625">优</th><th width="167.7991943359375">劣</th><th>适用场景</th></tr></thead><tbody><tr><td>多进程</td><td><ol><li>进程独享地址空间，相互独立，所以子程序崩溃不会影响主程序</li><li>通过增加 CPU 资源就可以扩充性能</li><li>没有线程锁这种复杂操作</li></ol></td><td>IPC逻辑复杂</td><td><ol><li>可靠性：进程不互相影响</li><li>分布式扩展容易</li></ol></td></tr><tr><td>多线程</td><td><ol><li>共享进程提供的代码和数据空间，不用跨越进程边界</li><li>程序逻辑简单</li><li>资源开销小、运行速度更快</li></ol></td><td><ol><li>受限于进程2GB的地址空间</li><li>线程间同步加锁较为复杂</li><li>程序崩溃可能导致整个程序异常</li></ol></td><td><ol><li>内存、CPU：上下文切换代价小</li><li>创建、销毁：资源开销小</li></ol></td></tr></tbody></table>

</details>

