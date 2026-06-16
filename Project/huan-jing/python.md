# Python

## MiniConda

{% tabs %}
{% tab title="使用" %}
```shellscript
conda env list

conda create -n $project python=3.10 -y
conda activate $project

conda remove --name $project --all
```
{% endtab %}

{% tab title="安装" %}
```bash
sudo yum install wget -y

wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

chmod +x Miniconda3-latest-Linux-x86_64.sh
./Miniconda3-latest-Linux-x86_64.sh
```
{% endtab %}

{% tab title="启动项" %}
```bash
echo 'export PATH="$HOME/miniconda3/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

conda init zsh
source ~/.zshrc
```
{% endtab %}
{% endtabs %}

***

