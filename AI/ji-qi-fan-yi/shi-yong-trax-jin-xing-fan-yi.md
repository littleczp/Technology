# 使用 Trax 进行翻译

Trax 包含一个可用于机器翻译任务的 Transformer 模型

## 安装 Trax

```sh
pip install -U trax
```

```python
import trax
```

## 创建原始 Transformer 模型

```python
model = trax.models.Transformer(
    ...
    d_model=512, ...
    n_heads=8, n_encoder_layers=6, n_decoder_layers=6...
    mode='predict'
)
```

## 使用预训练权重初始化模型

```python
model.init_from_file(..., weight_only=True)
```

## 对句子词元化

```python
sentence = 'I am only machine...'
tokenized = list(trax.data.tokenize(iter([sentence]),...))[0]
```

## 从 Transformer 解码

```python
tokenized = tokenized[None, :]
tokenized_translation = trax.supervised.decoding.autoregressive_sample(...)
```

## 对翻译结果去词元化并展示

```python
tokenized_translation = tokenized_translation[0][:-1]
translation = trax.data.detokenize(tokenized_translation, ...)
```
