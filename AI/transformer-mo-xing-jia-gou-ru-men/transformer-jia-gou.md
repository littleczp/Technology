# Transformer 架构

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="563"><figcaption><p>原始 Transformer 模型架构</p></figcaption></figure>

左侧是6层的编码器堆叠，右侧是6层的解码器堆叠。

输入进入左侧的编码器，穿过多头自注意力子层以及前馈神经网络子层，然后其目标输出进入右侧解码器的多头注意力子层和前馈神经网络子层
