# 飞行记录

Shell

{% code overflow="wrap" %}
```sh
uvicorn app.main:app --host 0.0.0.0 --port 8800 --reload --reload-dir ./app/
```
{% endcode %}

pm2

{% code overflow="wrap" %}
```sh
pm2 start uvicorn --name lumi-ai --interpreter /.../miniconda3/envs/lumi-ai/bin/python3.10 -- --host 0.0.0.0 --port 8188 --workers 2 app.main:app
```
{% endcode %}

ngrok

```sh
# 查看配置
cat ~/.config/ngrok/ngrok.yml
```

github lfs

```sh
git lfs track "app/backend/models/**"
git lfs track "app/backend/ffmpeg/**"
git add .gitattributes
```

## 优化

<details>

<summary>UserWarning: 1Torch was not compiled with flash attention</summary>

**安装支持Flash Attention的PyTorch，**&#x4F7F;用**CUDA 11.8+**&#x7248;本才能支持

| 配置               | 速度     | 显存占用     |
| ---------------- | ------ | -------- |
| 无Flash Attention | 1x     | 1x       |
| 有Flash Attention | 2-3x更快 | 减少30-50% |

</details>

<details>

<summary>whisperx</summary>

1. 避免免内存溢出：调整batch\_size=8、chunk\_length=30（每30秒分段）

</details>

<details>

<summary>升级Qwen模型</summary>

{% code overflow="wrap" %}
```python
import os

from modelscope import snapshot_download

# https://huggingface.co/collections/Qwen/qwen3-67dd247413f0e2e4f653967f
snapshot_download('qwen/Qwen3-4B',
                  local_dir=f'{os.path.join(BASE_DIR, "app", "services", "models", "LLM", "Qwen3-4B")}')
```
{% endcode %}

{% code overflow="wrap" %}
```python
text = tokenizer.apply_chat_template(
        msg,
        tokenize=False,
        add_generation_prompt=True,
        enable_thinking=False
    )

    generation_config = {
        "max_new_tokens": 512,
        "do_sample": False,
        "temperature": 0.1,
        "top_p": 0.9,
        "repetition_penalty": 1.05,
        "pad_token_id": tokenizer.eos_token_id,
        "eos_token_id": tokenizer.eos_token_id,
    }

    with torch.no_grad():
        model_inputs = tokenizer([text], return_tensors="pt").to("cuda")

        generated_ids = tokenizer_model.generate(
            model_inputs.input_ids,
            attention_mask=model_inputs.attention_mask,
            **generation_config
        )

        generated_ids = [
            output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
        ]

        response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]

        # 后处理：移除可能的多余空格和换行
        response = ' '.join(response.split())

        return response
```
{% endcode %}

</details>

***

## 告警

```python
import warnings

warnings.filterwarnings("ignore", category=FutureWarning, message=".*resume_download.*")
warnings.filterwarnings("ignore", category=UserWarning, module="pyannote.audio")
```

***

## 报错

<details>

<summary>whisperx/asr.py -> urllib.error.HTTPError: HTTP Error 301: Moved Permanently</summary>

VAD\_SEGMENTATION\_URL has Expired\
[issue](https://github.com/m-bain/whisperX/issues/943)\
[solved](https://github.com/m-bain/whisperX/pull/944/files)



解决：替换load\_vad\_model函数

```python
def load_vad_model(device, vad_onset=0.500, vad_offset=0.363, use_auth_token=None, model_fp=None):
    main_path = os.path.abspath(sys.argv[0])
    project_path = os.path.dirname(main_path)
    model_fp = os.path.join(project_path, "services/models/ASR/whisper", "pytorch_model.bin")
    model_bytes = open(model_fp, "rb").read()
    if hashlib.sha256(model_bytes).hexdigest() != VAD_SEGMENTATION_URL.split('/')[-2]:
        raise RuntimeError(
            "Model has been downloaded but the SHA256 checksum does not not match. Please retry loading the model."
        )

    vad_model = Model.from_pretrained(model_fp, use_auth_token=use_auth_token)
    hyperparameters = {"onset": vad_onset,
                       "offset": vad_offset,
                       "min_duration_on": 0.1,
                       "min_duration_off": 0.1}
    vad_pipeline = VoiceActivitySegmentation(segmentation=vad_model, device=torch.device(device))
    vad_pipeline.instantiate(hyperparameters)

    return vad_pipeline
```

</details>

<details>

<summary>Library libcublas.so.12 is not found</summary>

如果cuda版本不是12.\* pip uninstall faster-whisper pip install faster-whisper==0.10.1

修改版本后参数需要适配，注释max\_new\_tokens、clip\_timestamps、hallucination\_silence\_threshold

</details>

<details>

<summary>Max retries exceeded with url: /jonatasgrosman/wav2vec2-large-xlsr-53-chinese-zh-cn/resolve/main/preprocessor_config.json</summary>

```sh
export HF_ENDPOINT=https://hf-mirror.com
然后再次运行
```

<pre class="language-sh"><code class="lang-sh">git clone 
https://huggingface.co/jonatasgrosman/wav2vec2-large-xlsr-53-chinese-zh-cn

cd wav2vec2-large-xlsr-53-chinese-zh-cn
git lfs pull

<strong># 修改whispherX\whisperx\alignment.py导入:
</strong>processor = Wav2Vec2Processor.from_pretrained(&#x3C;model_name>)
align_model = Wav2Vec2ForCTC.from_pretrained(&#x3C;model_name>, local_files_only=True)
</code></pre>

</details>

<details>

<summary>RuntimeError: Library cublas64_12.dll is not found or cannot be loaded</summary>

```sh
ctranslate2==3.24.0 -> requirements
```

</details>

<details>

<summary>AttributeError: 'Wav2Vec2Processor' object has no attribute 'sampling_rate'</summary>

{% code overflow="wrap" %}
```sh
vim /root/miniconda3/envs/linly_dubbing/lib/python3.10/site-packages/whisperx/alignment.py
```
{% endcode %}



{% code overflow="wrap" %}
```python
from transformers import Wav2Vec2ForCTC, Wav2Vec2Processor, Wav2Vec2FeatureExtractor

if preprocess and model_type == 'huggingface': 
    processor = Wav2Vec2FeatureExtractor.from_pretrained(DEFAULT_ALIGN_MODELS_HF[model_lang])
```
{% endcode %}

</details>

<details>

<summary>KeyError: 'qwen2'</summary>

install `transformers>=4.37.0`

</details>

<details>

<summary>RuntimeError: Library libcublas.so.11 is not found</summary>

conda install cudatoolkit==11.8

</details>
