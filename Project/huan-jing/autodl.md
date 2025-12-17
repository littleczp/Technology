# AutoDL

## Conda

```shellscript
conda create --prefix ~/autodl-tmp/tmp/lumi-ai python=3.10 -y
conda activate ~/autodl-tmp/tmp/lumi-ai
```

autodl写入 \~/.bashrc 可以自动加载conda

```bash
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

设置conda下载缓存

```bash
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

***

## 模型

```shellscript
modelscope download qwen/Qwen3-4B --local_dir ./app/services/models/LLM/Qwen3-4B
```

