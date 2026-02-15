# 持久化

<details>

<summary>Redis服务器默认会创建16个数据库</summary>

Redis客户端的目标数据库为0号数据库，但客户端可以通过执行 SELECT 命令来切换目标数据库

</details>



<details>

<summary>过期键删除策略</summary>

1. 惰性删除：程序只会在取出键时才对键进行过期检查
2. 定期删除

</details>



