# BERT 的架构

BERT 将双向注意力机制引入 Transformer 模型中

## 编码器堆叠

BERT 只使用了 Transformer 编码器部分，没有使用解码器部分。BERT 包含 N=12 个编码器层，d<sub>model</sub>=768，多头注意力子层包含 A=12 个头（每个注意力头 Z<sub>A</sub> 的尺寸与原始 Transformer相同，都为64=768/12）

尺寸和纬度越大，预训练大型 Transformer 模型就越能更好地处理下游 NLP 任务

### 准备预训练环境

```
The cat sat on it because it was a nice rug.
```

假设当前位置为单词 it，则编码器的输入为：

```
The cat sat on it<masked sequence>
```

这种方法的目的是防止模型看到它应该预测的输出，但这个方法的缺点是模型无法学习到很多东西，模型想要弄清楚 it 指的是什么就必须要看到完整句子（rug）。所以 BERT 提出双向注意力，即一个注意力头从左向右，另一个注意力从右向左注意所有单词。

***

BERT 模型通过两项任务进行训练

1. 掩码语言建模（Masked Language Modeling，MLM）
2. 下一句预测（Next Sentence Prediction，NSP）

### 掩码语言建模

与原始 Transformer 模型对当前单词后续部分进行掩码不同，BERT 改为对句子进行双向分析，随机对句子中某一单词进行随机掩码

```
The cat sat on it [MASK] it was a nice rug.
```

这样多层注意力子层就可以看到整个序列，运行自注意力过程，然后预测被掩码的词元

### 下一句预测

NSP 是指输入将包含两个句子。在 50% 的情况下，第二个句子是真实的第二个句子；在另外 50% 的情况下，第二个句子的选择是随机的。NSP 添加了两个新词元：

* 【CLS】词元：CLS 是一个二分类词元，用于添加到第一个句子的开头，用于预测第二个句子是否跟随第一个句子
* 【SEP】词元：SEP 是一个分隔符词元，用于添加到每个句子的结尾

```
The cat slept on the rug. It likes sleeping all day.
```

添加词元后：

```
[CLS]The cat slept on the rug [SEP] It likes sleeping all day[SEP]
```
