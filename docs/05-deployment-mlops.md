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

## 📚 核心知识精讲（Made-With-ML + ML 系统设计提炼）

> GokuMohandas 的 Made-With-ML 和 Chip Huyen 的《ML 系统设计》是这一阶段的两座大山。我把其中**把 AI 应用真正送上线并稳定运行**必须掌握的核心提炼在这里。

### 1. 把应用变服务：FastAPI 四件套

Made-With-ML 用 FastAPI 做服务化，你要掌握这四点：

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Query(BaseModel):          # ① 用 Pydantic 定义请求体，自动校验参数
    question: str

@app.post("/ask")                # ② 定义接口路由和方法
def ask(q: Query):
    answer = my_rag_app(q.question)
    return {"answer": answer}    # ③ 返回 JSON，自动序列化

@app.get("/health")              # ④ 健康检查接口（部署/监控必备）
def health():
    return {"status": "ok"}
# 启动: uvicorn main:app --host 0.0.0.0 --port 8000
# 自带交互文档: 访问 /docs 就能测试接口
```

> **健康检查接口是刚需**：几乎所有部署平台（K8s、负载均衡）都靠定时请求 `/health` 判断你的服务是否活着。

### 2. Docker 容器化：三个核心概念 + 一个模板

**为什么要 Docker？** —— 解决"我电脑上能跑，服务器上跑不起来"的环境地狱。它把代码+依赖+环境打包成一个到哪都一致运行的"集装箱"。

- **Image（镜像）**：打包好的模板（只读）。
- **Container（容器）**：镜像跑起来的实例。
- **Dockerfile**：描述"怎么打包"的脚本。

```dockerfile
FROM python:3.11-slim            # 基础环境
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt   # 装依赖（先装依赖再拷代码，利用缓存加速）
COPY . .
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```
```bash
docker build -t my-ai-app .      # 打包成镜像
docker run -p 8000:8000 my-ai-app # 跑起来
```

### 3. 部署方式怎么选（按省心程度排序）

| 方式 | 省心度 | 适合 |
|---|---|---|
| **Serverless / Pages**（EdgeOne、Vercel、Cloudflare） | ⭐⭐⭐⭐⭐ | 小应用、个人项目，免运维、按量付费、自动伸缩 |
| **PaaS**（Railway、Render） | ⭐⭐⭐⭐ | 想要点控制权但不想碰服务器 |
| **云服务器 + Docker**（腾讯云/阿里云/AWS） | ⭐⭐⭐ | 需要完全控制、长驻服务 |
| **K8s 集群** | ⭐⭐ | 大规模、多服务、要弹性伸缩的生产系统 |

> 学习期建议直接上 **Serverless**，一条命令部署完拿到公网 URL，把精力放在打通闭环上。

### 4. CI/CD：让"改代码→上线"自动化

Made-With-ML 强调的工程习惯。核心：代码一 push，自动跑测试、自动构建镜像、自动部署。

```
git push → [CI] 跑测试+lint → 构建 Docker 镜像 → [CD] 自动部署到线上
```
- 工具：GitHub Actions（最易上手）、GitLab CI。
- 价值：告别"手动部署忘了某步导致线上炸了"。

### 5. 上线不是终点：监控与 MLOps 闭环

Chip Huyen 反复强调——**模型上线后才是挑战的开始**。要盯这几类指标：

| 类别 | 盯什么 |
|---|---|
| **系统指标** | 延迟(latency)、吞吐(QPS)、错误率、资源占用 |
| **模型指标** | 预测准确率、用户满意度、人工介入率 |
| **数据漂移** | 线上数据分布 vs 训练数据分布变了没 → 变了要重训 |

**MLOps 完整闭环**（这一阶段的终极图景）：
```
数据 → 训练 → 评估 → 部署 → 监控 → 发现漂移/收集反馈 → 回到训练
  ↑______________________ 持续迭代，模型越用越好 ______________________↓
```

### 6. LLM 应用特有的部署考量（做大模型应用必知）

传统 MLOps 之外，大模型应用还要额外关注：
- **成本控制**：API 按 token 计费，要监控用量、加缓存（相同问题不重复调用）、必要时限流。
- **延迟优化**：用流式输出(stream)改善体感；小任务用小模型。
- **提示词版本管理**：Prompt 也是"代码"，要版本化、能回滚、能 A/B 测试。
- **安全护栏**：输入过滤（防注入）、输出审核（防有害内容）。

---

### 📎 延伸参考

- **GokuMohandas/Made-With-ML**（⭐16.7k）：上面所有工程实践的完整实战教程，**首选精学**。
- **chiphuyen/dmls-book**（⭐8.6k）：《ML 系统设计》配套，监控/数据漂移/系统设计的"圣经"。
- **visenger/awesome-mlops**（⭐12k+）：MLOps 工具与文章大全，按需查。
- **部署平台**：EdgeOne Makers / Vercel / Cloudflare Workers（Serverless）；腾讯云/阿里云/AWS（服务器）。

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
