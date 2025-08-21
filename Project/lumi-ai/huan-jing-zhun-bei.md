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

### 子依赖

```sh
git submodule add https://github.com/FunAudioLLM/CosyVoice.git CosyVoice
git config --file .gitmodules --list
git commit -m "feat: add CosyVoice submodule"

git submodule update --init --force --recursive
git submodule update --init --recursive

# 如果match-TTS出错，需要清理缓存 & CosyVoice文件夹 & .gitmodules
rm -rf .git/modules/CosyVoice
# win
Remove-Item -Path .git\modules\CosyVoice -Recurse -Force
```

{% code overflow="wrap" %}
```sh
pip install --upgrade pip setuptools wheel

# 国内
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/

# autodl记得切tmp，不然系统盘不够
mkdir -p ~/autodl-tmp/tmp
export TMPDIR=~/autodl-tmp/tmp
export PIP_CACHE_DIR=~/autodl-tmp/tmp

# 通过子模块的 setup.py 安装包
pip install -r requirements_module.txt
# （代理）安装子模块
pip install -r requirements_module.txt -i https://pypi.org/simple --trusted-host pypi.org
# 如果总是超时
pip 加上 --default-timeout=1000 --retries 10

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

sttn（字幕）

{% file src="../.gitbook/assets/infer_model.pth" %}

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

# indextts(windows)
huggingface-cli download IndexTeam/IndexTTS-1.5 config.yaml bigvgan_discriminator.pth bigvgan_generator.pth bpe.model dvae.pth gpt.pth unigram_12000.vocab --local-dir .\app\services\models\TTS\IndexTTS-1.5

# indextts(linux)
modelscope download IndexTeam/IndexTTS-1.5 --local_dir ./app/services/models/TTS/IndexTTS-1.5
```
{% endcode %}
