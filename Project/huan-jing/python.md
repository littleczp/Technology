# Python

## MiniConda

安装

```bash
sudo yum install wget -y

wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

chmod +x Miniconda3-latest-Linux-x86_64.sh
./Miniconda3-latest-Linux-x86_64.sh
```

启动

```bash
echo 'export PATH="$HOME/miniconda3/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

conda init zsh
source ~/.zshrc
```

使用

```bash
conda env list

conda create -n $project python=3.10 -y
conda activate $project

conda remove --name lumi-ai --all
```

***

## Nginx

配置

```bash
vim /etc/nginx/nginx.conf
vim /etc/nginx/conf.d/lumipath.conf

server {
    listen 80;
    server_name lumipath.cn;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /openapi.json {
        proxy_pass http://localhost:8000/openapi.json;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }


    location /debug {
        rewrite ^/debug/(.*)$ /$1 break;
        proxy_pass http://localhost:8000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
    
    location /n8n/ {
        proxy_pass http://localhost:5678;
        rewrite ^/n8n/n8n/(.*)$ /n8n/$1 permanent;
        rewrite ^/n8n/(.*)$ /$1 break;
        proxy_redirect / /n8n/;

        # 标准头信息
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # 注意：支持 WebSocket (n8n 编辑器必须)
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_set_header X-Forwarded-Prefix /n8n;

        # 增加超时时间，防止视频处理时间过长导致 Nginx 504 报错断开
        proxy_read_timeout 3600s;
        proxy_send_timeout 3600s;
    }
}
```

运行

```bash
sudo nginx -t
sudo systemctl reload nginx
```

***

n8n脚本导出

```shellscript
chmod -R 777 local-media
docker compose exec n8n n8n export:workflow --all --output=/data/workflows_backup.json
```
