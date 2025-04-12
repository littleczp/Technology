# Stable Diffusion

<details>

<summary>扩散模型</summary>

在前向阶段对图像逐步施加噪声，直至图像被破坏变成完全的高斯噪声，然后在逆向阶段学习从高斯噪声还原为原始图像的过程

&#x20;

前向阶段：在原始图像X0上逐步增加噪声，每一步得到的图像Xt只和上一步的结果Xt-1相关，直至第T步的图像Xt变为纯高斯噪声



逆向阶段：不断去除噪声的过程, 首先给定高斯噪声Xt, 通过逐步去噪, 直至最终将原图像X0给恢复出来

&#x20;

模型训练完成后, 只要给定高斯随机噪声, 就可以生成一张从未见过的图像

</details>

Diffusion 模型的目标函数即是学习高斯噪声∈t和∈0(来自模型输出) 之间的 MSE loss

{% tabs %}
{% tab title="Mask Blur" %}
蒙版羽化：控制遮罩图像的模糊程度，值在 0-64 之间调节



数值较小的时候，边缘越锐利，数值一般默认4即可

对于更换背景图这样的场景，一般建议设置为0
{% endtab %}

{% tab title="Mask content" %}
在 Denoising 足够大时，使用latent noise更能体现 prompt

| latent noise             | latent nothing           |
| ------------------------ | ------------------------ |
| 潜在噪声                     | 无潜在空间                    |
| Mask flur 偏大时，背景会侵蚀到人物身上 | Mask flur 偏大时，背景会侵蚀到人物身上 |
{% endtab %}
{% endtabs %}
