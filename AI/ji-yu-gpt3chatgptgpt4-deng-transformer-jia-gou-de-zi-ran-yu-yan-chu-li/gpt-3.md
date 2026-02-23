# GPT-3

<details>

<summary> 零样本实验  </summary>

不对模型的参数进行微调，直接将预训练模型用于下游任务



目的：让预训练模型直接进入多任务生产

</details>

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

