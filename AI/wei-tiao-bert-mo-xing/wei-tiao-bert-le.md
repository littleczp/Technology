# 微调 BERT（略）

## 安装 Hugging Face PyTorch 接口

```sh
pip -q install transformers
```

## 导入模块（p50，略）

***

## 指定 Torch 使用 CUDA

```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
```

***

## 加载数据集（p51，略）

***

## 创建句子、标注列表以及添加【CLS】和【SEP】词元

***

## 激活 BERT 词元分析器

初始化一个预训练 BERT 词元分析器

***

## 处理数据

将数据集中的句子进行填充，补齐到统一的长度（128）

***

## 防止模型对填充词元进行注意力计算

通过掩码值来区分是否填充词元

***

## 将数据拆分为训练集和验证集

***

## 将所有数据转换为 torch 张量

```python
# torch tensors
train_inputs = torch.tensor(train_inputs)
validation_inputs = torch.tensor(validation_inputs)

train_labels = torch.tensor(train_labels)
validation_labels = torch.tensor(validation_labels)

train_masks = torch.tensor(train_masks)
validation_masks = torch.tensor(validation_masks)
```

***

## 选择批量大小并创建迭代器

```python
batch_size = 32
```

***

## BERT 模型配置

```python
from transformers import BertModel, BertConfig

configuration = BertConfig()
# init
model = BertModel(configuration)

config = model.config
print(config)
```

***

## 加载 Hugging Face BERT uncased base 模型

```python
model = BertForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=2)
model = nn.DataParallel(model)
model.to(device)
```

***

## 优化器分组参数

在进行模型微调的过程中，首先需要初始化预训练模型已学到的参数值

{% hint style="success" %}
微调一个预训练模型时，通常会使用已训练好的模型作为初始模型。因为它经过大量数据和计算资源训练，学到了很多有用的特征表示和参数权重

所以会使用预训练模型的参数值来初始化优化器，以便于在微调过程中更好地利用已经学到的参数，加快模型收敛速度并提高微调效果
{% endhint %}

{% code overflow="wrap" %}
```python
param_optimizer = list(model.named_parameters())
# the BERT 没有 "gamma" or "beta" parameters, only "bias"
no_decay = ["bias", "LayerNorm.weight"]
...
```
{% endcode %}

***

## 训练循环的超参数

<mark style="color:blue;">学习率(lr)和预热率(warmup)应该在优化阶段的早期设置为一个非常小的值</mark>，在一定迭代次数后逐渐增加。这样可以<mark style="color:red;">避免过大的梯度和超频问题</mark>

这里使用 BERT 版本的 Adam 优化器 — BertAdam

***

## 训练循环

```python
epochs = 4
```

***

## 对训练进行评估
