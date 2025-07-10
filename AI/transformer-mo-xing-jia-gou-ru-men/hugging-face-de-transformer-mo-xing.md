# Hugging Face 的 Transformer 模型

```sh
pip -q install transformers

# 国内
export HF_ENDPOINT=https://hf-mirror.com
```

```python
from transformers import pipeline

translator = pipeline(
    "translation_en_to_fr", 
    model="Helsinki-NLP/opus-mt-en-fr"
)
print(translator("It is easy to translate languages with transformers"))
```
