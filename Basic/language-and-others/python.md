# Python

<details>

<summary>为什么设计GIL锁？</summary>

GIL锁：同一时刻只有一个线程可以执行Python字节码

GIL锁的存在是为了保证同一时间内只有一个线程在运行，从而防止多个线程同时访问 Python 对象，造成内存管理问题，从而保证数据的安全性

</details>
