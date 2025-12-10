# wifi

## 网段对齐

> 独立局域网解决方案

{% tabs %}
{% tab title="路由器（交换机）" %}
将网口插入 WAN / 将猫插入 LAN


{% endtab %}

{% tab title="N1盒子" %}
将 N1 盒子的网口插入路由器 LAN 口


{% endtab %}

{% tab title="PC" %}
**配置 IPv4**

```
ip: 192.168.100.10
子网掩码: 255.255.255.0
dns: 192.168.100.1 (路由器)
```
{% endtab %}
{% endtabs %}

## Xray 版本

```shellscript
https://github.com/XTLS/Xray-core/releases/download/v25.12.8/Xray-linux-arm64-v8a.zip
```

