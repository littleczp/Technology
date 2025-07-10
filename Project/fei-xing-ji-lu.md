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

