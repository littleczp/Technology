---
description: Retrieval Augmented Generation
---

# RAG

## 什么是RAG？

RAG（检索增强生成） = 检索技术 + LLM 提示

通过自有垂域数据库检索相关信息，合并成为提示模板，给大模型润色生成回答

## 为什么需要RAG？

通用的基础大模型基本无法满足实际业务需求

* 知识的局限性：模型自身的知识完全源于它的训练数据，一些实时性的、非公开的或离线的数据是无法获取到的。
* 幻觉问题：所有的AI模型的底层原理都是基于数学概率，其模型输出实质上是一系列数值运算，所以它经常会一本正经地胡说八道，尤其是在大模型自身不具备某一方面的知识或不擅长的场景。
* 数据安全性：对于企业来说，数据安全至关重要，没有企业愿意承担数据泄露的风险，将自身的私域数据上传第三方平台进行训练。这也导致完全依赖通用大模型自身能力的应用方案不得不在数据安全和效果方面进行取舍。



\
