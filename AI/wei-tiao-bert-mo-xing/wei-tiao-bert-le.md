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

