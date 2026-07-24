# 1️⃣ 阶段一：编程与数学基础

> **目标**：能看懂代码、能看懂公式。为后面的一切打地基。
> **预计时长**：1-3 周（有编程基础可缩短）
> **产出**：跑通你人生中第一个机器学习模型。

---

## 🎯 本阶段你要达到的状态

- 能读写基础 Python（变量、循环、函数、类、库的导入）
- 会用 NumPy / Pandas 处理数据
- 理解机器学习最核心的思想：**从数据里学规律**
- 对线代/概率/微积分有"够用的直觉"（不要求会做题）

---

## 📖 核心概念

### 机器学习到底在干什么？

传统编程：`人写规则 + 数据 → 结果`
机器学习：`数据 + 结果 → 机器自己找出规则（模型）`

举例：判断邮件是不是垃圾邮件。
- 传统做法：人手写一堆规则（含"中奖"就是垃圾…），累且不准。
- ML 做法：喂给机器 10 万封"已标注是否垃圾"的邮件，它自己学出规律。

### 三类机器学习

| 类型 | 数据 | 例子 |
|---|---|---|
| 监督学习 | 带标签（有答案） | 房价预测、图片分类 |
| 无监督学习 | 无标签 | 用户分群、异常检测 |
| 强化学习 | 靠奖励信号 | 游戏 AI、机器人 |

### 一个模型的生命周期

```
准备数据 → 选模型 → 训练 → 评估 → 调优 → 使用(推理)
```

---

## 🔗 推荐学习项目

| 项目 | Star | 为什么适合你 |
|---|---|---|
| [microsoft/ML-For-Beginners](https://github.com/microsoft/ML-For-Beginners) | ⭐ 73.9k | **首选**。微软官方 12 周课，重直觉轻数学，每课配 Python 练习 + 小测验，零编程门槛 |
| [microsoft/AI-For-Beginners](https://github.com/microsoft/AI-For-Beginners) | ⭐ 38.6k | 12 课覆盖 CV、NLP、强化学习，讲 AI 如何"从数据学习" |
| [tangyudi/Ai-Learn](https://github.com/tangyudi/Ai-Learn) | 中文 | 中文实战就业路线图，近 200 个案例，Python→数学→ML→DL 一条龙 |

### Python 速成（如果完全不会编程）

- 官方教程：https://docs.python.org/zh-cn/3/tutorial/
- 廖雪峰 Python 教程（中文、免费、适合零基础）

### 数学补给（用到再看，别死磕）

- 3Blue1Brown《线性代数的本质》《微积分的本质》（B站有中文，视觉化神作）
- 可汗学院概率统计入门

---

## ✋ 动手任务（必做，做完才进阶段二）

1. **环境搭建**：安装 Python + Jupyter Notebook（或直接用 Google Colab，免安装）
2. **跟做 ML-For-Beginners 前 4 课**：了解回归、分类
3. **完成一个分类项目**：比如用鸢尾花数据集（Iris）训练一个能区分花种类的模型
   - 加载数据 → 划分训练/测试集 → 训练 → 看准确率
4. **✅ 交付物**：一个能跑的 `.ipynb`，输入花的特征，输出预测的花种类，并打印准确率

```python
# 你的第一个 ML 模型大概长这样（心里有个数）
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier

X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

model = DecisionTreeClassifier()
model.fit(X_train, y_train)              # 训练
print("准确率:", model.score(X_test, y_test))  # 评估
```

---

## 🚦 进入下一阶段的标志

- [ ] 能独立写出上面那段代码并跑通
- [ ] 能说清楚"训练集/测试集为什么要分开"
- [ ] 对"损失""准确率"这些词不再陌生

达标后 → 进入 [阶段二：模型原理·从零手写](02-model-principles.md)
