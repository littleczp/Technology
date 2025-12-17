# env

## SSH

{% tabs %}
{% tab title="生成密钥" %}
```shellscript
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
cat ~/.ssh/id_rsa.pub

ssh-keygen -t rsa -b 4096 -C "Lumi" -f ~/.ssh/Lumi
ssh-keygen -t rsa -b 4096 -C "x-lumi-fe" -f ~/.ssh/x-lumi-fe
```
{% endtab %}

{% tab title="管理多个仓库" %}
```shellscript
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
{% endtab %}
{% endtabs %}

