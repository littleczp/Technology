# GPT-3

## 上下文大小和最长距离

Transformer 模型的基石在于注意力子层。注意力子层的关键在于上下文大小

上下文越大，就越能理解呈现给我们的序列。但上下文越大，理解一个单词所指对象需要的距离就越长。

***

GPT的诞生：使用无监督学习来训练 Transformer 模型。然后使用监督学习对模型进行微调。

{% stepper %}
{% step %}
### 微调（Fine-Tuning）

先训练一个 Transformer 模型，然后在下游任务上进行微调
{% endstep %}

{% step %}
### 少样本（Few-Shot）

训练好一个Transformer 模型后，它能根据任务给出的少量样本作为条件进行推理
{% endstep %}

{% step %}
### 单样本（One-Shot）

训练好一个Transformer 模型后，它能根据任务给出的一个样本作为条件进行推理
{% endstep %}

{% step %}
### 零样本（Zero-Shot）

不需要任何样本即可执行下游任务
{% endstep %}
{% endstepper %}

GPT（Generative Pre-trained Transformer）模型只有一个编码器堆叠：**Decoder-only 架构**

三种主流大模型

1.  Decoder-only（只用解码器，单向预测）：极度擅长"文本生成（NLG）"和"长文本续写"。算力堆得越高，涌现出的通用智能越强。

    代表作：GPT 系列、Llama、Qwen、Gemini
2. Encoder-only（只用编码器，双向理解）：看句子时没有掩码，能够同时结合上下文（左边和右边的词）来理解当前词。极度擅长"文本理解（NLU）"

* 代表作：BERT 系列。文本分类、情感分析、搜索引擎的意图识别。但不具备生成新句子的能力。

3. Encoder-Decoder（完全体）：适合 Seq2Seq（序列到序列）的特定任务，比如把长文章变成短摘要，或者把英文变成中文。

* 代表作：T5、BART、传统机器翻译模型。目前已经逐渐被 Decoder-only 边缘化。
