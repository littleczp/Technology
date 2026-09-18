# 网站SEO和AEO

AEO（AI Engine Optimization）：依赖底层的搜索 API → AI 总结 + 检索增强生成（RAG）

{% code title="网站SEO和AEO Prompt" overflow="wrap" %}
```md
目前网站在 Google、Baidu 和 AI 搜索引擎（Perplexity、ChatGPT、Claude）中均未建立有效认知与收录。

请以“资深 Web 架构师与 SEO/AEO 专家”的身份，帮我深度审查当前仓库代码，重点关注以下几个方面，并指出缺陷和具体的修改方案：

0. 参考
​
1. 爬虫与站点地图：
 - 检查 `/robots.txt`，确保允许谷歌、百度、以及主要 AI 爬虫（GPTBot、PerplexityBot、ClaudeBot 等）正常访问。
 - 检查 `/sitemap.xml`，确保有正确的动态/静态站点地图配置，且 URL 是以 `nonocai.com` 开头的绝对路径。

2. TDK 与元数据规范：
 - 检查 `app/layout.tsx` 和各个 `page.tsx` 的 `Metadata` 配置。
 - 确认是否配置了标准的 `title`、`description`、`canonical` 规范化标签
​
3. AEO 与 JSON-LD：
 - 为首页和核心页面编写 schema.org 结构化数据。比如 WebSite, SoftwareApplication。
 - 不要使用 FAQPage，该富媒体结果自 2023 年起 Google 已基本停用，收益接近零。
 - 所有被标记的内容必须在页面上真实存在。不要标记不存在的信息

4. 可索引内容缺口
 - 审查发现核心页面缺少可被索引的正文，指出来并提出新增独立页面的方案（/docs、/changelog、/about、用例页等）。
 - 不要建议往现有页面里塞文字。

修改要求：
1. 不新增客户端 JS，不引入第三方脚本，不增加网络请求。
3. 不在现有页面里插入为 SEO、AEO 而写的内容。不要动首页和核心产品页。

## 输出
按"缺陷 → 影响 → 具体改法（含文件路径）"的格式。
把改动按"收益/成本"排序，告诉我哪些最值得先做。
```
{% endcode %}



## 检查项

1. 源码中是否有真实内容
2. 有无 sitemap.ts 和 robots.ts（robot.txt）
3. Layout 有无 metadata



## SEO优化

1. Google：[search.google.com/search-console](https://search.google.com/search-console) 中登记域名（Cloudfare可关联直接认证）
2. Google 中添加站点地图：[https://nonocai.com/sitemap.xml](https://nonocai.com/sitemap.xml)



## AEO优化

1. 加入结构化 JSON-LD
