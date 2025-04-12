# System Design

<details>

<summary>如何设计复杂和大规模系统</summary>

1. 分治：把问题空间分割为规模更小且易于处理的若干子问题，即高内聚低耦合的子系统
2. 抽象：精简问题，将问题变得容易理解

</details>

## DDD

<details>

<summary>Domain Driven Design：领域驱动设计</summary>

<table><thead><tr><th width="242.3333740234375">层级</th><th>作用</th></tr></thead><tbody><tr><td>User Interface，用户层</td><td>展示信息和传入用户命令。也可以是外部系统</td></tr><tr><td>Application，应用层</td><td>协调多个领域服务，完成User Case</td></tr><tr><td>Domain，领域层</td><td>表达业务概念</td></tr><tr><td>Infrastructure，基础实施层</td><td>提供公共的基础设施组件，如DB、消息、外部系统调用等</td></tr></tbody></table>

</details>

