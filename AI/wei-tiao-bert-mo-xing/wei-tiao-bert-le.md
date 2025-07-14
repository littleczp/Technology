# 微调 BERT（略）

## 安装 Hugging Face PyTorch 接口

```sh
pip -q install transformers
```

## 导入模块（p50，略）

## 指定 Torch 使用 CUDA

```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
```

## 加载数据集（p51，略）

## 创建句子、标注列表以及添加【CLS】和【SEP】词元

## 激活 BERT 词元分析器

初始化一个预训练 BERT 词元分析器

## 处理数据
