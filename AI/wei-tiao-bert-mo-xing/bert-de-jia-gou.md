# BERT 的架构

BERT 将双向注意力机制引入 Transformer 模型中

## 编码器堆叠

BERT 只使用了 Transformer 编码器部分，没有使用解码器部分。BERT 包含 N=12 个编码器层，d<sub>model</sub>=768，多头注意力子层包含 A=12 个头（每个注意力头 Z<sub>A</sub> 的尺寸与原始 Transformer相同，都为64=768/12）

尺寸和纬度越大，预训练大型 Transformer 模型就越能更好地处理下游 NLP 任务
