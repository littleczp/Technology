# 零拷贝

## 概念

减少在传输过程中不必要的数据复制（**在内存之间，不需要 CPU 参与数据的搬运**）



## 原理

### 文件服务器发送文件

```c
// 1. 从硬盘读取数据到内存
read(file, buf, len);
// 2. 把内存里的数据发给网卡
write(socket, buf, len);
```

{% stepper %}
{% step %}
### DMA 拷贝

硬盘 -> 内核缓冲区（Kernel Buffer）。(硬件负责，CPU 不参与)
{% endstep %}

{% step %}
### CPU 拷贝

内核缓冲区 -> 用户缓冲区（User Buffer）。(痛点1：CPU 亲自搬运，且发生了从内核态到用户态的切换)
{% endstep %}

{% step %}
### CPU 拷贝

用户缓冲区 -> Socket 缓冲区（也是内核态）。(痛点2：数据又搬回内核态，CPU 再次搬运)
{% endstep %}

{% step %}
### DMA 拷贝

Socket 缓冲区 -> 网卡。(硬件负责)
{% endstep %}
{% endstepper %}

```c
sendfile(socket, file, len); // ≥ Linux 2.1
```

{% stepper %}
{% step %}
### DMA 拷贝

硬盘 -> 内核缓冲区 (DMA)。(硬件负责，CPU 不参与)
{% endstep %}

{% step %}
### DMA Gather

网卡直接通过 DMA，去**内核缓冲区**里拿数据
{% endstep %}
{% endstepper %}
