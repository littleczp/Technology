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

