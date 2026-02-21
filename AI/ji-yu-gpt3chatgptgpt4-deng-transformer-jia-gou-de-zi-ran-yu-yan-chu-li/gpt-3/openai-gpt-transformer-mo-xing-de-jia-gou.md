# OpenAI GPT Transformer 模型的架构

## Transformer 模型扩大历史

GPT-3

* 模型层数：从原始 Transformer 的 6 层增加到 GPT-3 模型的96 层
* 每层的头数：从原始 Transformer 模型的 8 个增加到 GPT-3 模型的96个
* 上下文大小：从原始 Transformer 模型的 512 个词元增加到 GPT-3 模型的 12288 个词元



## 上下文大小和最长距离

Transformer 模型的基石在于注意力子层。注意力子层的关键在于上下文大小

上下文越大，就越能理解呈现给我们的序列。但上下文越大，理解一个单词所指对象需要的距离就越长。

