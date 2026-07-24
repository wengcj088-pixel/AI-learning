# 3️⃣ 阶段三：生成式 AI 应用入门

> **目标**：会用大模型做出真正的东西。
> **预计时长**：2-3 周
> **产出**：一个基于大模型 API 的对话应用 + 一次 Prompt 工程实验。

---

## 🎯 本阶段你要达到的状态

- 会调用大模型 API（OpenAI / 本地模型 / 国产模型）
- 掌握 Prompt Engineering（提示词工程）的核心技巧
- 理解 few-shot、思维链等常用 Prompt 模式
- 能做一个简单但完整的对话/生成类小应用

---

## 📖 核心概念

### 从"训练模型"到"使用模型"

阶段二你懂了模型怎么来的。这一阶段起，你**不用再自己训练**——直接用别人训练好的强大模型（GPT-4、Claude、Llama、Qwen 等），通过 API 或本地部署来使用。

### Prompt 工程：和模型对话的艺术

同样一个模型，Prompt 写得好不好，输出天差地别。核心技巧：

| 技巧 | 说明 | 例子 |
|---|---|---|
| **清晰指令** | 明确告诉模型要做什么、输出什么格式 | "用 3 个要点总结，每点不超过 20 字" |
| **角色扮演** | 给模型设定身份 | "你是一位资深 Python 工程师…" |
| **Few-shot** | 给几个示例让模型照做 | 给 2-3 个"输入→输出"样例 |
| **思维链 (CoT)** | 让模型"一步步想" | "让我们一步步推理…" |
| **结构化输出** | 要求返回 JSON 等格式 | "以 JSON 返回，字段为 name/age" |

### 大模型的能力边界（要心里有数）

- ✅ 擅长：语言理解、生成、总结、翻译、代码、创意
- ⚠️ 短板：会**幻觉**（编造事实）、不知道训练截止后的新知识、不擅长精确计算
- 👉 这些短板正是阶段四（RAG / 工具调用）要解决的

---

## 🔗 推荐学习项目

| 项目 | Star | 亮点 |
|---|---|---|
| [microsoft/generative-ai-for-beginners](https://github.com/microsoft/generative-ai-for-beginners) | ⭐ 95.9k | **首选**。微软 21 节实战课，Prompt 工程、微调、语义搜索全覆盖，全部可在 Colab 免费跑 |
| [HandsOnLLM/Hands-On-Large-Language-Models](https://github.com/HandsOnLLM/Hands-On-Large-Language-Models) | ⭐ 16.7k | "图解版 LLM 教程"，近 300 张图解，从 pipeline 到部署 |
| [dair-ai/Prompt-Engineering-Guide](https://github.com/dair-ai/Prompt-Engineering-Guide) | ⭐ 50k+ | 最全的 Prompt 工程指南，技巧、论文、案例齐全 |

---

## ✋ 动手任务（必做）

1. **跑通第一个 API 调用**：用任意大模型 API 完成一次对话
2. **做一个对话小应用**：用 Streamlit / Gradio 快速搭一个网页版聊天机器人
   - 输入框 + 对话历史 + 调用大模型返回
3. **Prompt 工程实验**：对同一个任务（如"总结一篇文章"），对比不同 Prompt 的效果，记录发现
   - ✅ **交付物**：一个能在浏览器里对话的小应用 + 一份 Prompt 对比笔记

```python
# 最小可用的大模型调用（以 OpenAI 兼容接口为例）
from openai import OpenAI

client = OpenAI(base_url="你的API地址", api_key="你的key")
resp = client.chat.completions.create(
    model="模型名",
    messages=[
        {"role": "system", "content": "你是一个乐于助人的助手"},
        {"role": "user", "content": "用一句话解释什么是注意力机制"}
    ]
)
print(resp.choices[0].message.content)
```

---

## 🚦 进入下一阶段的标志

- [ ] 能独立调用大模型 API 并处理返回
- [ ] 有一个能在浏览器里跑的对话小应用
- [ ] 能说出至少 3 种 Prompt 技巧并举例
- [ ] 理解为什么模型会"幻觉"

达标后 → 进入 [阶段四：RAG / Agent 工程实战](04-rag-agent-engineering.md)
