# Agent

## Agent的思考🤔

### 为什么需要引入agent这个概念？

从“解释”问题到“解决”问题

{% hint style="success" %}
大模型只有基本的多轮对话问答能力，没有真正的Step by Step解决问题的能力
{% endhint %}

### agent能够给业务带来什么价值？

Agent能做的事情，在Agent出现之前其实就能做。并且Agent还面临着许多挑战

* 降低应用开发门槛：不必懂代码
* 简化流程的复杂度：把用户的问题输入内容自然地转换到相应的API入参上，自动完成逻辑校验
* 完成复杂任务：不必按照工作流一尘不变的解决问题

{% hint style="warning" %}
Agent面临的挑战

* 响应速度慢
  * 硬件：芯片级提升，提升GPU性能
  * 软件：
    * 框架：FlashAttention、vLLM
    * 推理速度：Transformer kv cache
    * 模型：模型参数裁剪、模型蒸馏
    * 工程：预处理（大文本大文档切分），prompt信息压缩
* 幻觉：大模型天然的设计问题，可能会产生事实性错误或不遵循指令的幻觉，更加引发了信任危机，对Agent执行结果的挑战就更大了
  * 规范化prompt
  * hidden-tought进行reasoning推理
  * graphAG引入知识图谱推理
* 纯文本交互不友好：输出阶段的时候很多Agent会有很多长篇大论的输出，啰里啰嗦，阅读起来比较费劲
{% endhint %}

## Agent 设计框架

$$
Agent = LLM + 规划 + 记忆 +工具使用
$$

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

* **规划（Planning）：**&#x4E3B;要包括**子目标分解、反思与改进**
* **记忆（Memory）：**&#x5206;为**短期记忆**和**长期记忆**

<details>

<summary>短期记忆</summary>

将所有的上下文学习（比如提示工程Prompt Engineering、情景学习In-Context Learning）都看作模型的短期记忆来学习

</details>

<details>

<summary>长期记忆</summary>

利用外部的向量存储和快速检索来存储和召回信息。为Agent提供了长期存储和召回信息的能力

</details>

* **工具（Tools）：**&#x8C03;用外部API来获取模型权重（通常在预训练后很难修改）中缺少的额外信息

<details>

<summary>缺少的额外信息</summary>

当前信息，代码执行能力，访问专有信息源等。

</details>

* **动作（Action）：**&#x6839;据上述的规划、记忆、工具，大模型才能决策出最终需要执行的动作是什么



