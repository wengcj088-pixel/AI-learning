# 2️⃣ 阶段二：模型原理 · 从零手写

> **目标**：打通"黑盒"，让你再也不怕 Transformer。
> **预计时长**：3-5 周
> **产出**：亲手写出一个能生成文本的 mini-GPT。

> ⚠️ **这是最关键、也最容易被跳过的一步。** 很多人跳过后遗憾终身，永远停在"调包侠"。
> 亲手敲一遍，你对 AI 的理解会发生质变。

---

## 🎯 本阶段你要达到的状态

- 理解神经网络是怎么"学"的（前向 + 反向传播 + 梯度下降）
- 能从零实现一个自动微分引擎（micrograd）
- 理解注意力机制（Attention）到底在算什么
- 能从零搭出一个简化版 GPT

---

## 📖 核心原理

### 神经网络怎么学？三步循环

```
1. 前向传播：输入 → 网络 → 预测输出
2. 算损失：  预测 vs 真实答案，差多少？
3. 反向传播：算出"每个参数该往哪调"，然后调一点点
        ↑___________________ 重复几万次 ___________________↓
```

### 反向传播 = 链式法则

反向传播听起来玄乎，本质就是高中学过的**链式求导**：
> 损失对每个参数的影响，可以从输出层一层层"传"回去算出来。

Karpathy 的 micrograd 用 100 行代码就把这件事讲透了。

### 注意力机制（Attention）—— Transformer 的灵魂

一句话：**处理某个词时，让模型能"看"句子里所有词，并决定该"关注"谁。**

例："那只猫没吃东西因为**它**太累了" —— 模型要判断"它"指的是"猫"还是"东西"，靠的就是注意力。

计算三件套：**Query（我在找什么）、Key（我是什么）、Value（我携带的信息）**
- 用 Query 和所有 Key 算相似度 → 得到"注意力权重"
- 用权重对 Value 加权求和 → 得到融合了上下文的新表示

### 从注意力到 GPT

```
输入文本 → Tokenize → Embedding → [ 多层 Transformer Block ] → 输出下一个词的概率
                                    每个 Block = 自注意力 + 前馈网络
```

GPT 做的事简单到离谱：**预测下一个词**。反复预测，就生成了整段文本。

---

## 🔗 推荐学习项目

| 项目 | Star | 为什么必学 |
|---|---|---|
| [karpathy/nn-zero-to-hero](https://github.com/karpathy/nn-zero-to-hero) | ⭐ 16.9k | **本阶段核心**。前特斯拉 AI 总监 Karpathy 亲授，从 micrograd 一路手写到 GPT，视频+代码，公认最佳入门 |
| [rasbt/LLMs-from-scratch](https://github.com/rasbt/LLMs-from-scratch) | ⭐ 21.5k | 配套《从零构建大语言模型》，逐行实现注意力/位置编码/预训练/微调，17 小时视频 |
| [labmlai/annotated_deep_learning_paper_implementations](https://github.com/labmlai/annotated_deep_learning_paper_implementations) | ⭐ 60k+ | 60+ 篇经典论文的逐行注释实现，想深入某个模型时的宝库 |

### Karpathy 的学习顺序（强烈建议照抄）

1. **micrograd** —— 100 行实现自动微分，彻底搞懂反向传播
2. **makemore** —— 从 bigram 到 MLP，理解语言模型基础
3. **建 GPT (nanoGPT)** —— 手写一个能生成莎士比亚风格文本的 GPT

---

## ✋ 动手任务（必做）

1. **手写 micrograd**：跟着 Karpathy 视频，亲手实现一个能自动求导的 `Value` 类
   - ✅ 交付物：能对 `a*b+c` 这样的表达式自动算出梯度
2. **手写 makemore**：训练一个能生成"像人名"的字符级模型
3. **手写 mini-GPT**：跟着 nanoGPT，训练一个能生成文本的小 GPT
   - ✅ **核心交付物**：喂给它一段文本（如莎士比亚），它能生成风格类似的新文本

```python
# 手写注意力的核心，大概长这样（心里有个数）
import torch, torch.nn.functional as F

def attention(Q, K, V):
    # Q,K,V 形状: (序列长, 维度)
    scores = Q @ K.transpose(-2, -1)          # 算相似度
    weights = F.softmax(scores / (K.size(-1)**0.5), dim=-1)  # 归一化成权重
    return weights @ V                         # 加权求和
```

---

## 🚦 进入下一阶段的标志

- [ ] 能用自己的话解释"反向传播在干什么"
- [ ] 手写的 micrograd 能跑通
- [ ] 手写的 mini-GPT 能生成文本（哪怕结果很烂也算成功）
- [ ] 能说清楚 Q/K/V 各自是干什么的

达标后 → 进入 [阶段三：生成式 AI 应用入门](03-genai-applications.md)

> 🎉 走到这里，你已经超过 90% 只会调 API 的人了。
