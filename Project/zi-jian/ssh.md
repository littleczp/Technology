# SSH

## SSH

{% tabs %}
{% tab title="生成密钥" %}
```shellscript
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
cat ~/.ssh/id_rsa.pub

ssh-keygen -t rsa -b 4096 -C "xxx" -f ~/.ssh/xxx
ssh-keygen -t rsa -b 4096 -C "xxx2" -f ~/.ssh/xxx2
```
{% endtab %}

{% tab title="管理多个仓库" %}
```shellscript
vim ~/.ssh/config

Host g1
    HostName github.com
    User git
    IdentityFile ~/.ssh/xxx
    IdentitiesOnly yes

Host g2
    HostName github.com
    User git
    IdentityFile ~/.ssh/xxx2
    IdentitiesOnly yes
```

Git操作

```bash
git clone git@g1:oversea-mcn/xxx.git
git clone git@g2:oversea-mcn/xxx2.git
```
{% endtab %}
{% endtabs %}

