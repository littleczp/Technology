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

clip通过对数据词元化、编码和嵌入进行对比学习（寻找差异和相似性）。

输入：图片+文本

输出：Image Embeddings、Text Embeddings、相似度矩阵

* Text Encoder（文本编码器）：通常是一个小型的 Transformer（类似 GPT/BERT），负责把文本变成 512 维向量。
* Image Encoder（图像编码器）：通常是一个 ViT（Vision Transformer）或 ResNet，负责把图片变成 512 维向量。
{% endstep %}

{% step %}
### DALL-E

语义解析（clip） → 跨空间映射（Prior） → 扩散去噪渲染（Diffusion Model）

1. 文生图
2. 图像编辑
   1. Inpainting：内补全，局部更新
   2. Outpainting：外补全
3. 图像变体生成：输入一张图，提取图的"核心语义和风格"，生成无数神似的"平替"图片
{% endstep %}
{% endstepper %}
