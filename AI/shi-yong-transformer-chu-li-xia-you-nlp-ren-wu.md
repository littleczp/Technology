# 使用Transformer处理下游NLP任务

Transformer 拥有将知识应用于它们没有学过的任务的独特能力

> 例如，BERT Transformer 通过序列到序列和掩码语言建模来获取基础语言能力。只需要对 BERT Transformer 进行微调，就可以执行下游任务

## Transformer性能与人类基准

Transformer可通过继承预训练模型的特性来执行下游任务。预训练模型通过参数提供架构和语言表示

### 评估模型性能的度量指标

1. 准确率分数：F1分数、马修斯相关系数（MCC）
2. 基准任务和数据集：GLUE、SUPRERGLUE
   1. 模型
   2. 数据集驱动型任务
   3. 度量指标（准确率分数）



## 执行下游任务

下游任务：从预训练 Transformer 模型继承模型和参数的微调任务



