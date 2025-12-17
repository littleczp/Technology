# 环境准备

## Conda

```sh
conda create -n lumi-ai python=3.10 -y

conda remove --name lumi-ai --all

conda create --prefix ~/autodl-tmp/tmp/lumi-ai python=3.10 -y
conda activate ~/autodl-tmp/tmp/lumi-ai
```

autodl写入 \~/.bashrc 可以自动加载conda

```
# 初始化 conda
if [ -f "/root/miniconda3/etc/profile.d/conda.sh" ]; then
    source "/root/miniconda3/etc/profile.d/conda.sh"
elif [ -f "/opt/conda/etc/profile.d/conda.sh" ]; then
    source "/opt/conda/etc/profile.d/conda.sh"
else
    export PATH="/root/miniconda3/bin:$PATH"
fi

# 激活指定环境（如果存在）
if conda env list | grep -q "autodl-tmp/tmp/lumi-ai"; then
    conda activate ~/autodl-tmp/tmp/lumi-ai
fi
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

```sh
#!/bin/bash

# 设置绝对路径
PIP_CACHE="~/autodl-tmp/tmp"
SYSTEM_CACHE="~/autodl-tmp/tmp"

# 创建目录
mkdir -p $PIP_CACHE
mkdir -p $SYSTEM_CACHE

# 设置环境变量
export PIP_CACHE_DIR=$PIP_CACHE
export XDG_CACHE_HOME=$SYSTEM_CACHE

# 永久保存
echo "export PIP_CACHE_DIR=\"$PIP_CACHE\"" >> ~/.bashrc
echo "export XDG_CACHE_HOME=\"$SYSTEM_CACHE\"" >> ~/.bashrc

# 设置pip配置
pip config set global.cache-dir "$PIP_CACHE"

echo "设置完成！"
echo "PIP_CACHE_DIR: $PIP_CACHE_DIR"
echo "XDG_CACHE_HOME: $XDG_CACHE_HOME"
```

{% tabs %}
{% tab title="子模块" %}
```sh
pip install -r requirements_module.txt

# autodl(venv)
pip install -r requirements_module.txt -i https://pypi.org/simple/
```
{% endtab %}

{% tab title="异常" %}
子模块安装submodules/tts时会出错：num2words，可以分开安装

```sh
pip install submodules/demucs/.
pip install submodules/whisper/.
pip install submodules/whisperX/.

# 使用conda来解决依赖num2words
conda install -c conda-forge num2words
# venv情况下可以替换源
pip install num2words -i https://pypi.org/simple/

pip install submodules/TTS/.
```

分布安装 pip install submodules/whisperX/. 时会报错：Collecting av==11.\* 安装异常

```sh
# 更新包列表
apt update

# 安装 FFmpeg 开发库
apt install -y \
    pkg-config \
    libavformat-dev \
    libavcodec-dev \
    libavdevice-dev \
    libavutil-dev \
    libavfilter-dev \
    libswscale-dev \
    libswresample-dev \
    ffmpeg
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

{% tab title="CosyVoice" %}
```sh
git submodule add --force https://github.com/FunAudioLLM/CosyVoice.git CosyVoice

cd CosyVoice
pip install -r requirements.txt
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
match-TTS出错，需要清理缓存 & CosyVoice文件夹 & .gitmodules

```sh
rm -rf .git/modules/CosyVoice
# win
Remove-Item -Path .git\modules\CosyVoice -Recurse -Force
```

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

### FFMPEG-GPU

出现以下输出说明支持gpu加速

```
ffmpeg -encoders | grep nvenc

...

 V....D h264_nvenc           NVIDIA NVENC H.264 encoder (codec h264)
 V..... nvenc                NVIDIA NVENC H.264 encoder (codec h264)
 V..... nvenc_h264           NVIDIA NVENC H.264 encoder (codec h264)
 V..... nvenc_hevc           NVIDIA NVENC hevc encoder (codec hevc)
 V....D hevc_nvenc           NVIDIA NVENC hevc encoder (codec hevc)
```

## 模型

镜像：[https://hf-mirror.com/](https://hf-mirror.com/)

多线程下载器

```sh
sudo apt update
sudo apt install aria2
```

whisperx

{% code overflow="wrap" %}
```sh
mkdir -p ~/autodl-tmp/lumi-ai/app/services/models
mkdir -p ~/autodl-tmp/lumi-ai/app/services/models/ASR && mkdir -p ~/autodl-tmp/lumi-ai/app/services/models/LLM

cd ~/autodl-tmp/lumi-ai/app/services/models/ASR
aria2c -x 16 https://download.pytorch.org/torchaudio/models/wav2vec2_fairseq_base_ls960_asr_ls960.pth
aria2c -x 16 https://github.com/m-bain/whisperX/raw/a83ddbdf9ba29278cd6de50f2d735df3cd3984b1/models/pytorch_model.bin
```
{% endcode %}

autodl下载模型

{% code overflow="wrap" %}
```powershell
modelscope download keepitsimple/faster-whisper-large-v3 --local_dir ./app/services/models/ASR/whisper/faster-whisper-large-v3

modelscope download AI-ModelScope/XTTS-v2 --local_dir ./app/services/models/TTS/XTTS-v2

modelscope download qwen/Qwen3-4B --local_dir ./app/services/models/LLM/Qwen3-4B

# funasr
modelscope download iic/speech_seaco_paraformer_large_asr_nat-zh-cn-16k-common-vocab8404-pytorch --local_dir ./app/services/models/ASR/FunASR/speech_seaco_paraformer_large_asr_nat-zh-cn-16k-common-vocab8404-pytorch

modelscope download iic/speech_fsmn_vad_zh-cn-16k-common-pytorch --local_dir ./app/services/models/ASR/FunASR/speech_fsmn_vad_zh-cn-16k-common-pytorch

modelscope download iic/punc_ct-transformer_cn-en-common-vocab471067-large --local_dir ./app/services/models/ASR/FunASR/punc_ct-transformer_cn-en-common-vocab471067-large

modelscope download iic/speech_campplus_sv_zh-cn_16k-common --local_dir ./app/services/models/ASR/FunASR/speech_campplus_sv_zh-cn_16k-common
```
{% endcode %}

indextts2

```sh
cd IndexTTS
modelscope download --model IndexTeam/IndexTTS-2 --local_dir checkpoints

```
