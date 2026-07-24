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

## 📚 核心知识精讲（Karpathy 体系 + LLMs-from-scratch 提炼）

> 这一节我把 Karpathy 的 nn-zero-to-hero 和 rasbt 的 LLMs-from-scratch 里**从零手写模型时真正要理解的每一块**都提炼出来了，按他们的学习顺序排好。跟着理解每一块"在算什么、为什么这么算"，再去敲代码就水到渠成。

### 第 1 块：micrograd —— 用 4 个概念讲透反向传播

Karpathy 的 micrograd 只有 ~100 行，但它把神经网络最核心的机制讲透了。核心就是造一个 `Value` 类，让每个数字都记住"我是怎么被算出来的"：

- **前向**：正常做运算 `d = a*b + c`，同时记录运算图（谁是谁的输入）。
- **local gradient（局部梯度）**：每个运算节点知道"我的输出对我每个输入的偏导"。比如 `d=a*b`，则 `∂d/∂a = b`、`∂d/∂b = a`。
- **链式法则**：把整条路径上的局部梯度**相乘再相加**，就得到最终损失对某个参数的梯度。
- **`.backward()`**：从输出往回，按拓扑序把梯度一路传回每个参数——这就是"反向传播"的全部。

```python
# micrograd 的灵魂：每个运算都定义"怎么把梯度传给输入"
def __mul__(self, other):          # self * other
    out = Value(self.data * other.data)
    def _backward():
        self.grad  += other.data * out.grad   # ∂out/∂self = other
        other.grad += self.data  * out.grad   # ∂out/∂other = self
    out._backward = _backward
    return out
```

> **顿悟点**：所谓"训练"，就是不停地 `前向算损失 → backward 算梯度 → 参数 -= 学习率*梯度`。深度学习框架（PyTorch）做的就是这件事，只是规模更大。

### 第 2 块：makemore —— 语言模型的三级进化

makemore 教你训练一个"生成像人名的字符串"的模型，让你理解语言模型的本质是**预测下一个字符/词**。它带你走三级台阶：

1. **Bigram（查表统计）**：只看前 1 个字符猜下一个，本质是统计"哪两个字母常连在一起"。让你理解语言模型 = 概率分布。
2. **神经网络版 Bigram**：把查表换成一个单层网络 + softmax，引出"用梯度下降学出这张表"。
3. **MLP（多层感知机）**：看前 N 个字符预测下一个，引入 Embedding、隐藏层。这就是 2003 年 Bengio 经典语言模型的复现。

**关键概念**：
- **Tokenization**：把文本切成模型能处理的单元（字符/子词）。
- **Embedding**：把每个 token 映射成一个向量，语义近的向量也近。
- **Softmax**：把一串分数变成"加起来为 1 的概率分布"，用来选下一个词。
- **交叉熵损失**：衡量"预测的概率分布"和"真实答案"差多少，训练目标就是让它变小。

### 第 3 块：注意力机制 —— 手写才能真懂

这是 Transformer 的灵魂。前面章节讲了 Q/K/V 的直觉，这里给出**自注意力**完整计算，你要能默写：

```python
# 自注意力核心（带因果掩码，GPT 用的就是这个）
import torch, torch.nn.functional as F

def self_attention(x, Wq, Wk, Wv):
    Q, K, V = x @ Wq, x @ Wk, x @ Wv          # 1. 每个 token 算出自己的 Q/K/V
    scores = Q @ K.transpose(-2, -1)          # 2. Q 和所有 K 点积 = 相关性打分
    scores = scores / (K.size(-1) ** 0.5)     # 3. 缩放，防止数值过大梯度不稳
    mask = torch.tril(torch.ones_like(scores))# 4. 因果掩码：只能看自己和前面的词
    scores = scores.masked_fill(mask == 0, float('-inf'))
    weights = F.softmax(scores, dim=-1)       # 5. 归一化成注意力权重
    return weights @ V                        # 6. 按权重融合所有 Value
```

**要理解的 6 个点**：为什么要 Q/K/V 三个矩阵、点积为什么代表相关性、为什么除以 √d、因果掩码为什么让 GPT 只能"往前看"、softmax 的作用、加权求和得到了什么。

### 第 4 块：从注意力拼出完整 GPT（nanoGPT / LLMs-from-scratch）

一个 GPT 就是把下面这些积木堆起来：

```
Token IDs
  → Token Embedding + Positional Embedding   （词义 + 位置信息）
  → N × Transformer Block:
        ├─ LayerNorm → 多头自注意力 → 残差连接
        └─ LayerNorm → 前馈网络(MLP)  → 残差连接
  → LayerNorm → 线性层 → 输出每个词的概率
```

必须理解的 4 个"为什么"：
- **多头注意力**：多组 Q/K/V 并行，让模型从不同角度关注（有的看语法、有的看指代）。
- **残差连接 (x + f(x))**：让梯度能顺畅传到深层，是能堆几十上百层的关键。
- **LayerNorm**：稳定每层的数值分布，训练更快更稳。
- **前馈网络 (MLP)**：注意力负责"融合信息"，MLP 负责"加工信息"，两者交替。

### 第 5 块：预训练 vs 微调（LLMs-from-scratch 后半重点）

- **预训练**：拿海量文本，让模型反复做"预测下一个词"，学到通用语言能力。烧钱、耗时，个人一般不做。
- **微调 (Fine-tuning)**：在预训练好的模型上，用少量特定数据继续训练，让它擅长某个任务（如客服、写代码）。
- **指令微调 / SFT**：用"指令→理想回答"的数据微调，让模型学会"听话"。
- **这是理解 ChatGPT 怎么来的关键**：预训练给知识，指令微调 + RLHF 给"会聊天、听指令"的能力。

---

### 📎 延伸参考（跟着视频亲手敲一遍，事半功倍）

- **karpathy/nn-zero-to-hero**（⭐16.9k）：上面第 1~4 块的视频+代码原版，从 micrograd 手写到 GPT，公认最佳。**强烈建议照它的顺序敲一遍**。
- **rasbt/LLMs-from-scratch**（⭐21.5k）：上面第 3~5 块的逐行实现，配套《从零构建大语言模型》，含 17 小时视频。
- **labmlai/annotated_deep_learning_paper_implementations**（⭐60k+）：想深入某个具体模型时，60+ 篇论文的逐行注释实现宝库。

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
