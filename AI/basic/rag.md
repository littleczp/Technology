---
description: Retrieval Augmented Generation
---

# RAG

<details>

<summary>什么是RAG？</summary>

RAG（检索增强生成，**Retrieval-Augmented Generation**） = 检索技术 + LLM 提示

通过自有垂域数据库检索相关信息，合并成为提示模板，给大模型润色生成回答

1. **检索 (Retrieval)**
2. **增强 (Augumented)**
3. **生成 (Generation)**

</details>



<details>

<summary>为什么需要RAG？</summary>

通用的基础大模型基本无法满足实际业务需求

* 知识的局限性：模型自身的知识完全源于它的训练数据，一些实时性的、非公开的或离线的数据是无法获取到的。
* 幻觉问题：所有的AI模型的底层原理都是基于数学概率，其模型输出实质上是一系列数值运算，当它不知道答案时，为了让句子通顺，它往往会编造事实。
* 数据安全性：对于企业来说，数据安全至关重要，没有企业愿意承担数据泄露的风险，将自身的私域数据上传第三方平台进行训练。这也导致完全依赖通用大模型自身能力的应用方案不得不在数据安全和效果方面进行取舍。
* 成本效益：让模型学会新知识的另一种方法是"微调"（Fine-tuning），但这很贵，而且很难维护，而外挂知识库的成本极低。

</details>
