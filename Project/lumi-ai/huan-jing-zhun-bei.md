# 环境准备

## Conda

```sh
conda create -n lumi-ai python=3.10 -y

conda remove --name lumi-ai --all
```

## GPU

### CUDA

{% code overflow="wrap" %}
```sh
wget -c https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run

sudo sh cuda_11.8.0_520.61.05_linux.run
```
{% endcode %}

### Torch

寻找合适版本：[https://pytorch.org/get-started/locally](https://pytorch.org/get-started/locally/%20cuda%2011.8)（cuda 11.8）

## 依赖库

{% tabs %}
{% tab title="子模块" %}
```sh
pip install -r requirements_module.txt

# 切换源
pip install -r requirements_module.txt -i https://pypi.org/simple/
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="额外模块" %}
```sh
git submodule update --init --force --recursive
git submodule update --init --recursive
```
{% endtab %}

{% tab title="IndexTTS" %}
```sh
git submodule add --force https://github.com/index-tts/index-tts.git IndexTTS
cd IndexTTS
git checkout main
git pull

pip install -e .[webui] 
pip install torch==2.8.0+cu128 torchaudio==2.8.0+cu128 -f https://mirrors.aliyun.com/pytorch-wheels/cu128
```
{% endtab %}

{% tab title="异常" %}
pip网络超时：

```sh
pip 加上 --default-timeout=1000 --retries 10

pip 加上 -i https://pypi.org/simple --trusted-host pypi.org
```

If this call came from a \_pb2.py file, your generated code is out of date and must be regenerated with protoc >= 3.19.0

```sh
pip uninstall protobuf
pip install protobuf==3.20.3
```
{% endtab %}
{% endtabs %}

{% code overflow="wrap" %}
```sh
# 有一些子模块的包版本要替换，ctranslate2
pip install -r requirements.txt
# （代理）安装
pip install -r requirements.txt -i https://pypi.org/simple --trusted-host pypi.org

pip install torch==2.3.1 torchvision==0.18.1 torchaudio==2.3.1 --index-url https://download.pytorch.org/whl/cu118

# windows上安装indextts的依赖需要单独安装pynini
conda install -c conda-forge pynini==2.1.6
pip install WeTextProcessing --no-deps
```
{% endcode %}

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
sudo apt install fonts-noto fonts-noto-cjk fonts-noto-core fonts-noto-extra fonts-noto-mono
sudo apt install fonts-dejavu
fc-list : family | grep -i noto
fc-list : family | grep -i dejavu

sudo fc-cache -fv
```

## 模型

镜像：[https://hf-mirror.com/](https://hf-mirror.com/)

多线程下载器

```sh
sudo apt update
sudo apt install aria2
```

indextts2

```sh
cd IndexTTS
modelscope download --model IndexTeam/IndexTTS-2 --local_dir checkpoints

```
