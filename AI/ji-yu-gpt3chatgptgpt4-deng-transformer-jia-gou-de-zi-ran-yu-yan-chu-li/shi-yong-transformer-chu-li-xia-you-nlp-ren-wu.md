# 使用Transformer处理下游NLP任务

<details>

<summary>评估模型性能的度量指标</summary>

* 算法层
  * 基础困惑度（PPL）：**越低越好**，模型在预测下一个词时，平均在 n 个词中犹豫
  * 下游任务表现（如 BLEU/ROUGE）：精确率、召回率
* 工程层
  * TTFT（首字延迟）：从用户发出 Prompt 到模型吐出第一个 Token 的时间
  * TPOT（解码延迟）：生成第一个 Token 之后，后续每个 Token 的平均生成时间
  * 并发吞吐量

</details>



