# 环境

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
```

***

## SSH

```bash

ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
cat ~/.ssh/id_rsa.pub

ssh-keygen -t rsa -b 4096 -C "Lumi" -f ~/.ssh/Lumi
ssh-keygen -t rsa -b 4096 -C "x-lumi-fe" -f ~/.ssh/x-lumi-fe
```

organization多个仓库

```bash
vim ~/.ssh/config

Host g1
    HostName github.com
    User git
    IdentityFile ~/.ssh/Lumi
    IdentitiesOnly yes

Host g2
    HostName github.com
    User git
    IdentityFile ~/.ssh/x-lumi-fe
    IdentitiesOnly yes
```

Git操作

```bash
git clone git@g1:oversea-mcn/Lumi.git
git clone git@g2:oversea-mcn/x-lumi-fe.git
```

***

## TMUX

```bash
sudo yum install -y tmux

tmux new -s czp

tmux ls
tmux attach -t czp
tmux kill-session -t czp
```

***

## Nginx

配置

```bash
vim /etc/nginx/nginx.conf

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
}
```

运行

```bash
sudo nginx -t
sudo systemctl reload nginx
```

***

## Chrome

下载

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm


sudo yum install -y ./google-chrome-stable_current_x86_64.rpm
google-chrome --version

# 包管理
sudo yum install google-chrome-stable -y
sudo yum remove google-chrome-stable
```

找版本

```bash
https://mirror.cs.uchicago.edu/google-chrome/pool/main/g/google-chrome-stable/

https://mirrors.aliyun.com/google-chrome/google-chrome/
wget https://mirrors.aliyun.com/google-chrome/google-chrome/google-chrome-stable-114.0.5735.90-1.x86_64.rpm

sudo yum install -y ./google-chrome-stable-114.0.5735.90-1.x86_64.rpm
```

Driver

> https://chromedriver.storage.googleapis.com/LATEST\_RELEASE 可以看到需要的driver版本https://chromedriver.storage.googleapis.com/index.html?path={114.0.5735.90(chrome版本)}/

```bash
https://developer.chrome.com/docs/chromedriver/downloads?hl=zh-cn

https://googlechromelabs.github.io/chrome-for-testing/#stable

chmod +x ~/lumi/Lumi/app/utils/chromedriver
~/lumi/Lumi/app/utils/chromedriver --verbose
```
