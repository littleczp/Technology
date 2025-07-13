# 环境准备

## Conda

```sh
conda create -n lumi-ai python=3.10 -y
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

{% code overflow="wrap" %}
```sh
pip install --upgrade pip setuptools wheel

# 国内
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/

# 通过子模块的 setup.py 安装包
pip install -r requirements_module.txt
# 有一些子模块的包版本要替换，ctranslate2
pip install -r requirements.txt

pip install torch==2.3.1 torchvision==0.18.1 torchaudio==2.3.1 --index-url https://download.pytorch.org/whl/cu118
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

## 模型

多线程下载器

```sh
sudo apt update
sudo apt install aria2
```

whisperx

{% code overflow="wrap" %}
```sh
mkdir -p ~/lumi-ai/app/services/models
mkdir -p ~/autodl-tmp/models/ASR && mkdir -p ~/autodl-tmp/models/LLM

cd ~/autodl-tmp/models/ASR
aria2c -x 16 https://download.pytorch.org/torchaudio/models/wav2vec2_fairseq_base_ls960_asr_ls960.pth
aria2c -x 16 https://github.com/m-bain/whisperX/raw/a83ddbdf9ba29278cd6de50f2d735df3cd3984b1/models/pytorch_model.bin
ln -s ~/autodl-tmp/models/ASR ~/lumi-ai/app/services/models
```
{% endcode %}

