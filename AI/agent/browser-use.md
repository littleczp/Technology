# Browser Use

* **GitHub**: [browser-use/browser-use](https://github.com/browser-use/browser-use)
* **官网**: [browser-use.com](https://browser-use.com/)
* **文档**: [docs.browser-use.com](https://docs.browser-use.com/introduction)

{% hint style="success" %}
将 Agents 与真实浏览器进行交互，轻松实现浏览器自动化
{% endhint %}

<figure><img src="../.gitbook/assets/image (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

## Environment

> python ≥ 3.11

```sh
pip install browser-use
```

```
playwright install
```

编辑.env文件 或者 .py写入环境变量

```python
# .env
OPENAI_API_KEY=...
ANTHROPIC_API_KEY=...

# .py
DASHSCOPE_API_KEY = "sk-xxx"
os.environ["OPENAI_API_KEY"] = DASHSCOPE_API_KEY
```

## 飞行记录

{% tabs %}
{% tab title="幻觉" %}
<table><thead><tr><th width="312.4163818359375">幻觉问题</th><th>原因</th><th>解决思路</th></tr></thead><tbody><tr><td><p></p><ol><li>在使用 write_file 工具时，模型猜测的参数错误，因此 Pydantic 抛出了 validation error。</li><li>报错后，模型试图重试，但逻辑被打断，导致执行了 unknown() 操作</li></ol></td><td>模型觉得这个任务很复杂，所以它自作主张决定先写一个 todo.md 文件来做计划（日志中显示："thinking": "I should create a checklist..."）</td><td><p>prompt确规则：禁止它"做计划"</p><pre class="language-python"><code class="lang-python">Rules:
    - No file creation. NO `write_file`.
</code></pre><p><img src="../.gitbook/assets/image.png" alt=""></p></td></tr><tr><td>在某些step时调用这个不存在的工具，导致一直阻塞和重试这个step</td><td>"幻觉"出了一个不存在的工具，类似 get_attribute 或 read_element 的工具来获取 href，但 browser-use <strong>并没有提供直接获取元素属性的工具</strong></td><td><p>明确告诉模型："不要尝试直接读取，而是运行 JavaScript 代码来提取链接"</p><pre class="language-python"><code class="lang-python">CRITICAL STEP.
    - Execute the 'execute_javascript' tool with the code below.
    - DO NOT use the 'send_keys' tool.
    - DO NOT use the 'click_element' tool.

    JS Code:
    """
    """
</code></pre><p><img src="../.gitbook/assets/image (1).png" alt=""></p></td></tr><tr><td>返回的数据出现"纰漏"，多出不存在数据</td><td>Agent 在执行完 JS 后，会调用 done() 工具来结束任务。在这一步，Agent 会根据记忆<strong>手打</strong>一份总结报告。在这个“复述”的过程中，大模型有时候会像人一样“嘴瓢”或者“手抖”</td><td>修改解析逻辑，<strong>优先匹配“纯净”的 JSON 结果</strong>，并且一旦找到 <strong>立即停止（Break）</strong>，防止被后面的废话覆盖<img src="../.gitbook/assets/image (4).png" alt=""></td></tr></tbody></table>
{% endtab %}

{% tab title="一些工作原理" %}
1. Agent 会调用一个“内容提取工具”（extract\_content 或类似）来提取内容，而这个工具的工作原理是把网页转换成 Markdown 文本。在 Markdown 文本模式下，网页背后的代码（如 href 属性）往往会被“清洗”掉，只剩下可见的文字（“下载视频”）
{% endtab %}
{% endtabs %}

## Demo

```python
import os

# Disable telemetry
os.environ["ANONYMIZED_TELEMETRY"] = "false"

browser_profile = BrowserProfile(
    locale='en-US',
    user_agent='Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',
    headless=True  # 无头
)

task_prompt = """
    ...
"""

DASHSCOPE_API_KEY = "sk-xxx"
os.environ["OPENAI_API_KEY"] = DASHSCOPE_API_KEY

agent = Agent(
    browser_profile=browser_profile,
    initial_actions=[{"open_tab": {"url": target_site}}],
    task=task_prompt,
    llm=ChatOpenAI(
            api_key=SecretStr(DASHSCOPE_API_KEY),
            base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",  # 阿里云
            model=model_name,
            temperature=0.1
        ),
    use_vision=False,
    max_actions_per_step=3,
)
```
