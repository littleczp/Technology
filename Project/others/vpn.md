# vpn

更新系统工具

```shellscript
yum update -y
yum install -y wget curl
```

运行一键安装脚本

{% code overflow="wrap" %}
```shellscript
wget -P /root -N --no-check-certificate "https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh" && chmod 700 /root/install.sh && /root/install.sh
```
{% endcode %}

关闭CentOS防火墙

```shellscript
# 临时
setenforce 0

# 永久
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

***

执行安装脚本

```shellscript
/root/install.sh
```

* 安装Xray-core
* 选择VLESS-REALITY-Vision（选项7）

***

<br>
