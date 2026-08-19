# FFMPEG

### FFMPEG

{% code overflow="wrap" fullWidth="false" %}
```sh
conda install ffmpeg==7.0.2 -c conda-forge

# 国内
conda install ffmpeg==7.0.2 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
```
{% endcode %}

### FFMPEG-字体

```sh
sudo apt update

sudo apt install fonts-noto fonts-noto-cjk fonts-noto-core fonts-noto-extra fonts-noto-mono
sudo apt install fonts-dejavu
sudo apt install fonts-noto-color-emoji

fc-list : family | grep -i noto
fc-list : family | grep -i dejavu

sudo fc-cache -fv
```
