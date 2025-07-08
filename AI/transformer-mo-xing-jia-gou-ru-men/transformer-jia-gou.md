---
description: Attention is All You Need
---

# Transformer 架构

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="563"><figcaption><p>原始 Transformer 模型架构</p></figcaption></figure>

左侧是6层的编码器堆叠，右侧是6层的解码器堆叠。

输入进入左侧的编码器，穿过多头自注意力子层以及前馈神经网络子层，然后其目标输出进入右侧解码器的多头注意力子层和前馈神经网络子层



注意力取代了随着两个单词的距离增加而需要增加参数的循环函数（RNN）。它是“单词到单词”的操作，即 token 到 token（词元）的操作。注意力机制将找出每个单词与序列中所有单词的相关性。

> 注意力将单词向量进行**点积操作**以得出一个单词与所有单词的关系，包括与自身的关系
