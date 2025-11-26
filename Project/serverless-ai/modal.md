# Modal

## Start

```shellscript
pip install modal
python3 -m modal setup
```

## Image

{% tabs %}
{% tab title="完整demo" %}
```python
image = (
    modal.Image.from_registry(
        "nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04",
        add_python="3.10"
    )
    .apt_install(
        # ffmpeg
        "ffmpeg", "libsm6", "libxext6", "wget", "fonts-noto-cjk",
        ...
    )
    .pip_install(
        "ffmpeg-python",
        "openai",
        ...
    )
    .run_function(download_models)
    .add_local_file("ocr.py", "/root/ocr.py")
)
```
{% endtab %}

{% tab title="基础镜像" %}
```python
image = (
    modal.Image.debian_slim()
)
```
{% endtab %}

{% tab title="依赖" %}
```python
image = (
    modal.Image.debian_slim()
    .apt_install(
        # ffmpeg
        "ffmpeg", "libsm6", "libxext6", "wget", "fonts-noto-cjk",
        # paddle
        "libgl1", "libglib2.0-0",
    )
    .uv_pip_install("transformers[torch]")
    .pip_install(...)
)
```
{% endtab %}

{% tab title="下载模型" %}
```python
image = (
    modal.Image.debian_slim()
    .run_function(download_models)
)

def download_models():
    # --- 1. 下载 Demucs 模型 (htdemucs_ft) ---
    from demucs.pretrained import get_model
    print("Downloading Demucs models...")
    get_model("htdemucs_ft")

    # --- 2. 下载 Whisper 模型 ---
    import whisper
    print("Downloading Whisper model...")
    whisper.load_model("large-v3")

    # --- 3. 下载 PaddleOCR 模型 ---
    print("Downloading PaddleOCR models...")
    from paddleocr import PaddleOCR

    # 初始化 PaddleOCR 会自动触发下载机制
    PaddleOCR(
        lang='ch',
        use_angle_cls=True,
        show_log=True,
        use_gpu=False
    )
    print("PaddleOCR models downloaded successfully.")
```
{% endtab %}
{% endtabs %}
