# 4️⃣ 阶段四：RAG / Agent 工程实战

> **目标**：做出真正能落地、能用的 AI 产品。
> **预计时长**：3-4 周
> **产出**：一个 RAG 文档问答系统 + 一个能调用工具的 Agent。

---

## 🎯 本阶段你要达到的状态

- 理解并实现 RAG（检索增强生成）
- 会用向量数据库做语义搜索
- 理解 Agent（智能体）的工作原理
- 会用 Function Calling / 工具调用扩展模型能力
- 能把上面的东西组合成一个完整应用

---

## 📖 核心原理

### RAG：解决"幻觉"和"不懂私有知识"

大模型不知道你公司的内部文档，也可能一本正经胡说。RAG 的思路：

```
用户提问
   │
   ▼
① 把问题转成向量 ──▶ ② 在向量库里检索最相关的资料
   │                          │
   ▼                          ▼
④ 大模型基于资料回答 ◀── ③ 把"问题 + 检索到的资料"一起喂给模型
```

关键：**先检索，再生成**。模型的回答有据可依，大幅减少幻觉。

### 向量数据库 & Embedding

- **Embedding**：把文本变成一串数字向量，语义相近的文本向量也相近。
- **向量数据库**：专门存这些向量，支持"给一个向量，找最相似的 N 个"。
- 常见：Chroma（轻量本地）、Milvus、Pinecone、pgvector。

### Agent：让 AI 会"行动"，不只是"回答"

普通对话：你问 → 它答。
Agent：你给目标 → 它**自己规划步骤、调用工具、多步执行**，直到完成。

```
目标 → [ 思考该做什么 → 调用工具 → 看结果 → 再思考 ] 循环 → 完成
                          ↑
              工具: 搜索/计算/查数据库/调 API
```

### Function Calling / Tool Use

让模型能调用你定义的函数。比如问"北京今天天气"，模型不会瞎编，而是**调用天气 API** 拿真实数据再回答。

---

## 🔗 推荐学习项目

| 项目 | Star | 亮点 |
|---|---|---|
| [microsoft/ai-agents-for-beginners](https://github.com/microsoft/ai-agents-for-beginners) | ⭐ 5k+ | 微软 11 章 Agent 入门课，含中文，从概念到 Azure 部署 |
| [ed-donner/llm_engineering](https://github.com/ed-donner/llm_engineering) | ⭐ 2k+ | 8 周训练营，会议纪要→RAG 问答→多智能体，渐进式项目 |
| [Arindam200/awesome-ai-apps](https://github.com/Arindam200/awesome-ai-apps) | ⭐ 2.5k+ | 100+ 可运行 AI 应用示例，按难度分级，一键部署 |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | ⭐ 90k+ | 最流行的 LLM 应用开发框架，RAG/Agent 都有现成组件 |

### 主流工具栈（挑一套上手即可）

- **框架**：LangChain / LlamaIndex（RAG 尤其好用）
- **向量库**：Chroma（本地起步最简单）
- **快速界面**：Streamlit / Gradio

---

## ✋ 动手任务（必做）

### 项目 A：RAG 文档问答系统

1. 准备一批文档（PDF / Markdown，比如某本书、某产品手册）
2. 切分文档 → 生成 Embedding → 存入向量库
3. 用户提问 → 检索相关片段 → 拼进 Prompt → 大模型回答
4. ✅ **交付物**：一个能回答"你自己那批私有文档"的问答应用

### 项目 B：工具调用 Agent

1. 定义 2-3 个工具（如：计算器、网页搜索、查数据库）
2. 让模型根据问题自动决定调用哪个工具
3. ✅ **交付物**：一个问"帮我搜一下 X 并总结"能自动完成的 Agent

```python
# RAG 的核心流程（伪代码，帮你建立心智模型）
chunks = split_document(my_docs)            # 1. 切分
vectors = embed(chunks)                     # 2. 向量化
db.add(vectors, chunks)                     # 3. 存入向量库

def answer(question):
    q_vec = embed(question)
    context = db.search(q_vec, top_k=3)     # 4. 检索最相关的3段
    prompt = f"根据以下资料回答：\n{context}\n\n问题：{question}"
    return llm(prompt)                      # 5. 基于资料生成答案
```

---

## 🚦 进入下一阶段的标志

- [ ] 有一个能跑的 RAG 问答系统（回答基于你的私有文档）
- [ ] 有一个能调用工具的 Agent
- [ ] 能解释 RAG 为什么能减少幻觉
- [ ] 理解 Embedding 和向量检索在做什么

达标后 → 进入 [阶段五：生产部署与 MLOps](05-deployment-mlops.md)

> 💪 到这里你已经能独立开发 AI 应用了。最后一步：让它真正上线，别人能用。
