# 📅 可执行学习计划（12 周闭环）

> 一份**照着走就行**的周计划。零基础全职学约 12 周，业余时间学则相应拉长。
> 核心原则：**每周都要动手产出一个能跑的东西。**

---

## 总览

| 周次 | 阶段 | 主题 | 周末产出 |
|:---:|:---:|---|---|
| 1-2 | 一 | Python + ML 基础 | 第一个分类模型 |
| 3 | 一 | 数据处理 + 评估 | 一份数据分析报告 |
| 4-5 | 二 | 神经网络 + 手写 micrograd | 能自动求导的引擎 |
| 6-7 | 二 | 注意力 + 手写 mini-GPT | 能生成文本的 GPT |
| 8 | 三 | 大模型 API + Prompt 工程 | 对话小应用 |
| 9 | 三 | Prompt 实验 + 结构化输出 | Prompt 对比笔记 |
| 10 | 四 | RAG 文档问答 | RAG 问答系统 |
| 11 | 四 | Agent + 工具调用 | 能用工具的 Agent |
| 12 | 五 | 部署上线 + 监控 | 公网可访问的 AI 应用 |

---

## 逐周详细计划

### 🟢 第 1-2 周：Python + 机器学习基础
- **看**：`ML-For-Beginners` 前 8 课
- **做**：装好 Colab/Jupyter；跟做每课练习
- **产出**：用 Iris 数据集训练分类模型，打印准确率
- **补**：不会 Python 就先花 3 天过廖雪峰教程

### 🟢 第 3 周：数据处理与模型评估
- **看**：`ML-For-Beginners` 剩余课程
- **做**：用 Pandas 处理一份真实数据集（如 Titanic）
- **产出**：一份含可视化的数据分析 notebook

### 🔵 第 4-5 周：神经网络原理 + micrograd
- **看**：Karpathy `nn-zero-to-hero` 的 micrograd + makemore
- **做**：**亲手敲** micrograd，不要复制粘贴
- **产出**：能对表达式自动求梯度的 `Value` 类；一个字符级 makemore

### 🔵 第 6-7 周：注意力机制 + mini-GPT
- **看**：Karpathy 的 nanoGPT 部分；`LLMs-from-scratch` 注意力章节
- **做**：手写自注意力；训练一个 mini-GPT
- **产出**：喂莎士比亚文本，能生成风格类似的新文本

### 🟡 第 8 周：大模型 API + Prompt 工程
- **看**：`generative-ai-for-beginners` 前 10 课
- **做**：跑通 API 调用；用 Streamlit 搭对话界面
- **产出**：一个浏览器里能对话的小应用

### 🟡 第 9 周：Prompt 工程深入
- **看**：`Prompt-Engineering-Guide`；`generative-ai-for-beginners` 剩余课
- **做**：对同一任务测试 few-shot / CoT / 结构化输出
- **产出**：一份 Prompt 效果对比笔记

### 🟠 第 10 周：RAG 文档问答
- **看**：`llm_engineering` RAG 部分；LlamaIndex 快速上手
- **做**：切分文档 → Embedding → Chroma → 检索 → 回答
- **产出**：能回答你私有文档的 RAG 问答系统

### 🟠 第 11 周：Agent 与工具调用
- **看**：`ai-agents-for-beginners` 前几章
- **做**：定义工具；实现 Function Calling；跑一个多步 Agent
- **产出**：一个"搜索并总结"能自动完成的 Agent

### 🔴 第 12 周：部署上线（闭环收尾）
- **看**：`Made-With-ML` 部署部分
- **做**：FastAPI 封装 → Dockerfile → 部署到 Serverless → 加日志
- **产出**：🎉 一个别人打开链接就能用的 AI 应用

---

## ✅ 打卡表（复制到你的笔记里用）

```
第1周  [ ] Python基础  [ ] ML前8课        产出:[ ] 分类模型
第2周  [ ] ML继续      [ ] 练习完成        产出:[ ] 准确率报告
第3周  [ ] Pandas      [ ] 数据可视化      产出:[ ] 分析notebook
第4周  [ ] micrograd看 [ ] micrograd写     产出:[ ] Value类
第5周  [ ] makemore    [ ] 训练跑通        产出:[ ] 字符模型
第6周  [ ] 注意力原理  [ ] 手写attention   产出:[ ] 注意力代码
第7周  [ ] nanoGPT     [ ] 训练mini-GPT    产出:[ ] 文本生成
第8周  [ ] API调用     [ ] Streamlit界面   产出:[ ] 对话应用
第9周  [ ] Prompt技巧  [ ] 对比实验        产出:[ ] 对比笔记
第10周 [ ] RAG流程     [ ] 向量库检索      产出:[ ] RAG问答
第11周 [ ] Agent原理   [ ] 工具调用        产出:[ ] Agent
第12周 [ ] FastAPI     [ ] Docker+部署     产出:[ ] 上线应用 🎉
```

---

## 💡 坚持下去的几条建议

1. **每周产出优先于进度**：宁可慢一点，也要把每周的"能跑的东西"做出来。
2. **公开学习**：把每周产出发到社交平台/写博客，倒逼自己完成。
3. **卡住先查词典**：遇到术语翻 [概念词典](00-concepts-glossary.md)，别硬钻牛角尖。
4. **原理阶段别偷懒**：第 4-7 周是分水岭，手写代码一定要亲手敲。
5. **找同伴**：加入 Datawhale 等学习社区，组队不容易放弃。

---

> 12 周后回头看，你会感谢现在开始动手的自己。加油 🚀
