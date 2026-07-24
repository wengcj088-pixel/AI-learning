# 5️⃣ 阶段五：生产部署与 MLOps

> **目标**：从"能在我电脑上跑"到"上线稳定、别人能用"。
> **预计时长**：2-3 周
> **产出**：把一个 AI 应用/模型部署上线，并加上监控。

> 🔑 **这一步让闭环真正闭合。** 模型/应用躺在 notebook 里不算数，上线能被人访问、能稳定运行，才是完整的 AI 能力。

---

## 🎯 本阶段你要达到的状态

- 会把模型/应用封装成 API 服务
- 会用 Docker 容器化
- 会部署到云服务器 / Serverless 平台
- 理解监控、日志、数据漂移等运维概念
- 理解完整的 MLOps 流程

---

## 📖 核心概念

### 从"应用"到"服务"

你的 RAG/Agent 应用现在只能你自己跑。要让别人用，得：

```
应用代码
   │
   ▼
① 封装成 API (FastAPI/Flask)  ──▶ 对外暴露一个 URL 接口
   │
   ▼
② 容器化 (Docker)             ──▶ 打包环境，到哪都能一致运行
   │
   ▼
③ 部署 (云服务器/Serverless)  ──▶ 挂到公网，别人可访问
   │
   ▼
④ 监控 + 日志                 ──▶ 出问题能及时发现
```

### MLOps 完整闭环

```
数据 → 训练 → 评估 → 部署 → 监控 → 收集反馈数据 → (回到训练)
  ↑________________________ 持续迭代 ________________________↓
```

这就是"闭环"的终极形态：上线后收集真实数据，反过来优化模型，形成正向循环。

### 关键运维概念

| 概念 | 说明 |
|---|---|
| **CI/CD** | 代码一改就自动测试、自动部署，不用手动折腾 |
| **监控** | 盯着延迟、错误率、准确率，异常报警 |
| **数据漂移** | 线上数据和训练数据分布变了 → 模型变差，要能发现 |
| **A/B 测试** | 两个版本同时上线，用真实流量对比 |
| **模型量化** | 压缩模型让它更小更快，适合部署 |
| **弹性伸缩** | 流量大时自动扩容，流量小时缩容省钱 |

---

## 🔗 推荐学习项目

| 项目 | Star | 亮点 |
|---|---|---|
| [GokuMohandas/Made-With-ML](https://github.com/GokuMohandas/Made-With-ML) | ⭐ 16.7k | **首选**。教你把模型从实验推向生产：分布式训练、MLflow、CI/CD、监控、A/B 测试 |
| [chiphuyen/dmls-book](https://github.com/chiphuyen/dmls-book) | ⭐ 8.6k | Chip Huyen《ML 系统设计》配套，端到端系统设计"圣经" |
| [visenger/awesome-mlops](https://github.com/visenger/awesome-mlops) | ⭐ 12k+ | MLOps 资源大全，工具/文章/课程分类整理 |

### 常用部署工具栈

- **API 框架**：FastAPI（现代、快、自带文档）
- **容器**：Docker + Docker Compose
- **Serverless / Pages**：EdgeOne Makers、Vercel、Cloudflare Workers（适合小应用，免运维）
- **云服务器**：腾讯云 / 阿里云 / AWS
- **监控**：Prometheus + Grafana，或平台自带监控

---

## ✋ 动手任务（必做，闭环收尾）

1. **API 化**：把阶段四的 RAG/Agent 应用用 FastAPI 包成一个 `/ask` 接口
2. **容器化**：写一个 `Dockerfile`，本地 `docker build` + `docker run` 跑通
3. **部署上线**：部署到任意平台（Serverless 最省心），拿到一个公网 URL
4. **加监控**：至少加上请求日志 + 基础的错误捕获
5. ✅ **最终交付物**：一个**别人打开链接就能用**的 AI 应用

```python
# 用 FastAPI 把应用变成服务，大概长这样
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Query(BaseModel):
    question: str

@app.post("/ask")
def ask(q: Query):
    answer = my_rag_app(q.question)   # 调用你阶段四的应用
    return {"answer": answer}

# 本地跑: uvicorn main:app --host 0.0.0.0 --port 8000
```

---

## 🚦 毕业标志（恭喜完成整个闭环！）

- [ ] 应用已封装成 API
- [ ] 已用 Docker 容器化
- [ ] 已部署上线，有一个公网可访问的 URL
- [ ] 加上了基础监控/日志
- [ ] 能画出完整的 MLOps 闭环流程图

---

## 🎓 你已经走完了完整闭环

```
基本概念 → 核心原理 → 应用开发 → 工程实战 → 生产部署
   ✅         ✅         ✅         ✅         ✅
```

从"什么都不会"到"能独立开发并部署 AI 应用"，你已经具备了完整的 AI 工程能力。

**接下来往哪走？**
- 想搞**算法研究**：深读 `annotated_deep_learning_paper_implementations`，追前沿论文
- 想做**AI 应用/创业**：多复刻 `awesome-ai-apps`，打磨产品
- 想进**大厂**：系统学 `dmls-book`，补齐系统设计能力

> 学 AI 没有终点，但你已经跑通了从 0 到 1 最难的一段。继续动手，持续闭环 🚀
