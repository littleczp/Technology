# Browser Use

* **GitHub**: [browser-use/browser-use](https://github.com/browser-use/browser-use)
* **官网**: [browser-use.com](https://browser-use.com/)
* **文档**: [docs.browser-use.com](https://docs.browser-use.com/introduction)

{% hint style="success" %}
将 Agents 与真实浏览器进行交互，轻松实现浏览器自动化
{% endhint %}

<figure><img src=".gitbook/assets/image.png" alt="" width="375"><figcaption></figcaption></figure>

## Environment

> python ≥ 3.11

```sh
pip install browser-use
```

```
playwright install
```

编辑.env文件

```
OPENAI_API_KEY=...
ANTHROPIC_API_KEY=...
```

## Demo

```
import asyncio

from langchain_openai import ChatOpenAI
from browser_use import Agent
from dotenv import load_dotenv

load_dotenv()

llm = ChatOpenAI(model="gpt-4o")

async def main():
    agent = Agent(
        task="打开 https://google.com，搜索browser use并找到它的官网",
        llm=llm,
    )
    result = await agent.run()
    print(result)

if __name__ == "__main__":
    asyncio.run(main())
```
