# Transformer视觉模型

Transformer多模态神经元将图像化为像素（单词），对图像编码后，将图像视为单词词元来处理。

{% stepper %}
{% step %}
### ViT（Vision Transformers）

Vit可以更好地捕捉图像中不同位置的关系，同时具备处理长距离上下文信息的能力

1. 将图片拆分为图块（n x n）：避免逐像素处理
2. 对图块进行线性投影：图块序列
3. 嵌入图块序列到Transformer架构
{% endstep %}

{% step %}
### CLIP（Contrastive Language-Image Pre-Training）

clip的输入是文本-图像对，通过对数据进行词元化、编码和嵌入后


{% endstep %}
{% endstepper %}
