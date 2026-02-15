# 持久化

<details>

<summary>Redis服务器默认会创建16个数据库</summary>

Redis客户端的目标数据库为0号数据库，但客户端可以通过执行 SELECT 命令来切换目标数据库

</details>



<details>

<summary>过期键删除策略</summary>

1. 惰性删除：程序只会在取出键时才对键进行过期检查
2. 定期删除



服务器以AOF持久化模式运行时，当过期键被惰性删除或者定期删除之后，程序会向AOF文件追加一跳DEL命令，来显式地记录该键已被删除

</details>



