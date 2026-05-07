---
title: 逼近论
aliases:
  - Approximation Theory
category:
  - 数学与形式基础
tags:
  - ai-foundations
  - math
  - approximation-theory
type: topic
status: stable
importance: important
version: v2.0
date: '2026-04-08'
---

# 逼近论与泛函分析（Approximation Theory and Functional Analysis）：从起源到 AI 的完整脉络

> **上帝视角**：逼近论回答的是“神经网络为什么具有表达能力”，泛函分析则提供了描述函数空间的语言。万能逼近定理说明单隐层网络对广泛的连续函数具有存在性层面的逼近能力，这是深度学习的重要理论起点；核方法则提供了另一条路径，用高维特征空间中的线性结构实现非线性拟合。从 Weierstrass 的多项式逼近到现代神经正切核理论，逼近论始终是连接纯数学与机器学习的重要桥梁，但它本身并不自动回答“是否易于训练”与“是否一定泛化良好”。

---

## 相关主题

- [[02-linear-algebra|线性代数]]：基函数展开、投影与核矩阵计算都依赖线性空间语言
- [[03-calculus-and-optimization|微积分与优化理论]]：表达能力要转化为可训练模型，还需要优化算法把参数找到
- [[04-information-theory|信息论]]：模型复杂度、压缩与泛化问题常与信息量和复杂度度量相联系
- [[07-topology-and-geometry|拓扑与几何]]：函数空间、流形结构与表示的几何复杂性彼此关联
- [[13-numerical-analysis|数值分析]]：核矩阵求解、近似计算与大规模训练都涉及数值稳定性与复杂度

## 1. 上帝视角：为什么 AI 需要逼近论

机器学习的本质是**函数逼近**：给定有限的输入-输出样本，学习一个函数 $f$ 使其在未见数据上也能给出合理预测。逼近论与相关理论通常回答四类不同的问题：

1. **存在性**：目标函数能否被某类模型逼近？（万能逼近定理）
2. **效率性**：达到给定精度需要多大的模型？（逼近速率、深度分离）
3. **可学习性**：即使理论上能逼近，优化过程能否把这样的参数找出来？（核方法、NTK、训练动态）
4. **泛化能力**：在训练集上逼近得好，是否意味着对新样本也成立？（统计学习理论、正则化、隐式偏置）

### 四类结论不要混为一谈

这四个问题在 AI 文献中经常被连成一条“理论保证链”，但它们实际上对应不同层面的结论：

- **万能逼近定理**解决的是“有没有这样的函数表示”，不保证参数规模经济，也不保证训练算法能找到。
- **逼近速率与深度分离**回答“深和宽哪个更省参数”，但不直接给出实际训练样本量。
- **核方法、NTK、训练动态分析**讨论的是“怎样更容易学到”，它们比纯存在性结论更接近训练过程。
- **泛化理论**讨论的是“学到之后能否迁移到未见数据”，这又是与表达能力不同的层次。

把这四层区分开，才能避免把“能表示”误读成“能训练”，再误读成“能泛化”。

### 逼近论与泛函分析的关系

虽然本篇标题包含"泛函分析"，但需要明确两者的关系：

**逼近论（Approximation Theory）**：研究"如何用简单函数逼近复杂函数"
- **核心问题**：给定函数类 `𝓕`（如多项式、神经网络），能否逼近目标函数 `f`？逼近误差有多大？
- **经典结果**：Weierstrass 定理、万能逼近定理、深度分离定理
- **在 AI 中的体现**：神经网络的表达能力、模型容量与泛化的权衡

**泛函分析（Functional Analysis）**：为逼近论提供严格的数学框架
- **核心贡献**：定义函数空间（Banach 空间、Hilbert 空间）、范数、内积等概念
- **关键工具**：再生核 Hilbert 空间（RKHS）、算子理论、紧致性
- **在 AI 中的体现**：核方法（SVM、高斯过程）的理论基础

**两者的分工**：
- 泛函分析提供"函数空间的语言"，逼近论使用这套语言研究"逼近问题"
- 没有泛函分析，逼近论缺乏严格的数学基础；没有逼近论，泛函分析在 AI 中缺少应用场景
- 在本篇中，泛函分析主要出现在核方法（3.4节）和 RKHS 理论中

### 与其他学科的关键连接点

| 学科 | 连接点 | 具体关系 |
|------|--------|----------|
| 微积分与优化 | 梯度下降 | 逼近目标通过损失函数最小化实现 |
| 线性代数 | 基函数展开 | 函数逼近 = 在基函数空间中的投影 |
| 概率论 | 统计学习理论 | 逼近误差 + 估计误差 = 泛化误差 |
| 拓扑学 | 紧致性 | Weierstrass 定理依赖紧致集上的连续性 |
| 信息论 | 逼近复杂度 | 函数类的度量熵决定逼近难度 |
| 泛函分析 | 函数空间 | RKHS 为核方法提供数学框架 |
| 计算机科学 | 计算复杂度 | 逼近精度与网络规模的权衡 |

---

## 2. 历史脉络

逼近论的发展可以划分为三个阶段：经典逼近论、泛函分析时期、以及与 AI 的融合。

### 2.1 经典逼近论（19 世纪）

- **Weierstrass (1885)**：证明了闭区间上的连续函数可以被多项式一致逼近。这是逼近论的奠基性定理，开启了"用简单函数逼近复杂函数"的研究范式。
- **Chebyshev (1854)**：提出最佳一致逼近问题，发展了 Chebyshev 多项式理论。最小化最大误差（minimax）的思想后来影响了对抗训练和 GAN。
- **Bernstein (1912)**：给出了 Weierstrass 定理的构造性证明（Bernstein 多项式），首次提供了显式的逼近方案。

### 2.2 泛函分析时期（20 世纪上半叶）

- **Stone (1937)**：将 Weierstrass 定理推广到一般的紧致 Hausdorff 空间上的连续函数代数（Stone-Weierstrass 定理）。这一抽象化为后来的万能逼近定理铺平了道路。
- **Banach (1932)**：建立了 Banach 空间理论，为函数逼近提供了严格的范数框架。
- **Aronszajn (1950)**：系统发展了再生核 Hilbert 空间（RKHS）理论，成为核方法的数学基础。
- **Kolmogorov (1957)**：证明了叠加表示定理——任何多元连续函数都可以表示为有限个一元函数的叠加。这被视为神经网络万能逼近能力的早期数学预言。

### 2.3 与 AI 的融合（1980 年代至今）

- **Cybenko (1989)**：证明了具有 sigmoid 激活函数的单隐层前馈网络可以逼近任意连续函数（万能逼近定理）。
- **Hornik, Stinchcombe & White (1989)**：独立证明了类似结果，并将其推广到更一般的激活函数。
- **Barron (1993)**：给出了单隐层网络的逼近速率估计，证明神经网络可以避免维度灾难。
- **Vapnik (1995)**：将核方法与统计学习理论结合，提出支持向量机（SVM）。
- **Rahimi & Recht (2007)**：提出随机特征方法，用随机采样逼近核函数。
- **Jacot, Gabriel & Hongler (2018)**：提出神经正切核（NTK）理论，揭示了无限宽网络与核方法的等价性。
- **Lu et al. (2017)**：证明了 ReLU 网络的万能逼近定理，宽度为 n+1 的网络即可逼近 n 维空间上的任意连续函数。

## 3. 核心知识点详解

### 3.1 Weierstrass 逼近定理

#### 数学定义

**定理（Weierstrass, 1885）**：设 $f: [a, b] \to \mathbb{R}$ 是连续函数，则对任意 $\varepsilon > 0$，存在多项式 $p(x)$ 使得：

$$\sup_{x \in [a,b]} |f(x) - p(x)| < \varepsilon$$

等价地，多项式在 $C([a,b])$（配备上确界范数的连续函数空间）中稠密。

#### Bernstein 多项式构造

Bernstein 给出了显式构造：

$$B_n(f; x) = \sum_{k=0}^{n} f\left(\frac{k}{n}\right) \binom{n}{k} x^k (1-x)^{n-k}$$

其中 $\binom{n}{k} x^k (1-x)^{n-k}$ 是 Bernstein 基多项式。当 $n \to \infty$ 时，$B_n(f; x) \to f(x)$ 一致收敛。

#### 直觉理解

Weierstrass 定理告诉我们：**光滑的简单函数（多项式）可以任意精确地模拟任何连续行为**。这与神经网络的哲学一脉相承——用大量简单计算单元的组合来逼近复杂映射。

#### AI 中的角色

- 为"函数逼近"范式提供了最早的理论合法性
- Bernstein 多项式的构造思想（加权组合基函数）直接启发了神经网络的架构设计
- 逼近速率 $O(1/\sqrt{n})$ 揭示了多项式逼近的局限性，推动了对更高效逼近方案的探索

---

### 3.2 万能逼近定理（Universal Approximation Theorem）

#### 数学定义

**定理（Cybenko, 1989）**：设 $\sigma: \mathbb{R} \to \mathbb{R}$ 是连续的 sigmoid 型函数（即 $\sigma(t) \to 1$ 当 $t \to +\infty$，$\sigma(t) \to 0$ 当 $t \to -\infty$）。则对任意紧致集 $K \subset \mathbb{R}^n$、任意连续函数 $f: K \to \mathbb{R}$ 和任意 $\varepsilon > 0$，存在整数 $N$、实数 $w_i, b_i, v_i$ 使得：

$$\sup_{x \in K} \left| f(x) - \sum_{i=1}^{N} v_i \sigma(w_i^T x + b_i) \right| < \varepsilon$$

其中：$K$ 是输入空间中的紧致集，$f$ 是目标连续函数，$N$ 是隐层神经元数量，$w_i, b_i, v_i$ 分别表示第 $i$ 个神经元的输入权重、偏置和输出系数，$\varepsilon$ 是允许的逼近误差。

#### Hornik 的推广

**定理（Hornik et al., 1989）**：上述结论对任何非常值的有界可测激活函数 $\sigma$ 都成立。更进一步，Hornik (1991) 证明了单隐层网络不仅可以逼近函数值，还可以逼近其任意阶导数。

#### ReLU 网络的万能逼近

**定理（Lu et al., 2017）**：宽度为 $n+1$ 的 ReLU 网络可以逼近 $\mathbb{R}^n$ 上任意 Lebesgue 可积函数。

ReLU 函数定义为：

$$\text{ReLU}(x) = \max(0, x)$$

ReLU 网络的逼近能力来源于其**分片线性**特性：$N$ 个 ReLU 神经元可以将输入空间划分为至多 $2^N$ 个线性区域。

#### 直觉理解

万能逼近定理的核心信息是：

1. **单隐层就够了**——理论上不需要深层网络
2. **但定理没说需要多少神经元**——可能需要指数级数量
3. **也没说如何找到这些参数**——只保证存在性，不保证可学习性

这就像说"字典里的词足以表达任何思想"——正确但不实用。深度学习的实践成功在于：深层网络用**多项式规模**的参数就能高效逼近，而浅层网络可能需要**指数级**参数。

#### AI 中的角色

- 为神经网络提供了**理论合法性**：不是经验上碰巧有效，而是数学上保证有效
- 推动了对**逼近效率**的研究：从"能不能逼近"转向"多快能逼近"
- 激发了对**深度优势**的理论探索

---

### 3.3 深度 vs 宽度——为什么深层网络更高效

#### 深度分离定理

**定理（Telgarsky, 2016）**：存在由深度为 $k$ 的 ReLU 网络表示的函数，任何深度为 $k' < k$ 的网络要逼近它，宽度必须是指数级的。

具体地，深度为 $k$ 的网络可以表示具有 $2^k$ 个振荡的函数，而深度为 $k'$ 的网络要逼近这样的函数，宽度至少需要 $2^{k-k'}$。

#### 组合爆炸论证

深层网络的高效性可以从**组合**角度理解。考虑一个深度为 $L$、每层宽度为 $w$ 的网络：

- 每层执行一次非线性变换，将输入空间折叠
- L 层网络可以产生指数级（约 $w^L$ 量级）的线性区域
- 而宽度为 W 的单隐层网络只能产生线性级（约 W 量级）的线性区域

要达到相同的表达能力（$w^L$ 个区域），浅层网络需要 $W = w^L$ 个神经元，参数量从 $O(wL)$ 暴增到 $O(w^L)$。

#### Kolmogorov 叠加定理的启示

**定理（Kolmogorov, 1957）**：任何定义在 $[0,1]^n$ 上的连续函数 $f$ 都可以表示为：

$$f(x_1, \ldots, x_n) = \sum_{q=0}^{2n} \Phi_q\left(\sum_{p=1}^{n} \psi_{q,p}(x_p)\right)$$

其中 Φ_q 和 ψ_{q,p} 都是一元连续函数。

这个定理表明：**多元函数可以精确分解为一元函数的有限叠加**。虽然 Kolmogorov 叠加中的内部函数可能高度不规则（不可微），但它预示了深层网络的分层计算思想。

#### 直觉理解

深度的优势类似于**数字的位值表示**：用 $n$ 位二进制数可以表示 $2^n$ 个不同的值。每增加一位（一层），表达能力翻倍。而如果只用一位（一层），就需要 $2^n$ 个不同的符号。

#### AI 中的角色

- 解释了为什么实践中深层网络（ResNet-152、GPT 等）远优于浅层宽网络
- 指导网络架构设计：**深度优先于宽度**
- 为残差连接（ResNet）提供了理论动机：深度带来表达力，残差连接解决优化困难

---

### 3.4 核方法与 RKHS

#### 核函数的定义

**定义**：设 X 是输入空间，函数 k 是一个从 X×X 到实数的映射，称为**正定核**（positive definite kernel），如果对任意 n 个点，Gram 矩阵是半正定的。

#### 核技巧（Kernel Trick）

核技巧的核心思想：存在特征映射 φ（映射到高维甚至无穷维空间），使得：

$$k(x, y) = \langle \phi(x), \phi(y) \rangle_{\mathcal{H}}$$

其中：
- k(x,y) 是核函数
- φ(·) 是把输入映射到特征空间的特征映射
- H 是对应的 Hilbert 空间
- 尖括号表示该空间中的内积

关键在于：**我们不需要显式计算 $\phi(x)$，只需要计算核函数 $k(x,y)$**。这避免了高维空间中的计算爆炸。

#### 常用核函数

| 核函数 | 表达式 | 特征空间维度 |
|--------|--------|-------------|
| 线性核 | $k(x,y) = x^T y$ | $d$（有限） |
| 多项式核 | $k(x,y) = (x^T y + c)^p$ | $\binom{d+p}{p}$（有限） |
| RBF（高斯）核 | $k(x,y) = \exp(-\gamma \|x-y\|^2)$ | $\infty$（无穷） |
| Laplace 核 | $k(x,y) = \exp(-\gamma \|x-y\|_1)$ | $\infty$（无穷） |

#### 再生核 Hilbert 空间（RKHS）

**定义（Aronszajn, 1950）**：设 H 是定义在 X 上的函数构成的 Hilbert 空间。如果对每个 x ∈ X，求值泛函在 H 上连续，则 H 称为再生核 Hilbert 空间。

RKHS 的**再生性质**：

$$f(x) = \langle f, k(\cdot, x) \rangle_{\mathcal{H}}$$

即函数在某点的值可以通过与核函数的内积"再生"出来。

直白地说，RKHS 解决的是“我们要在哪个函数空间里讨论逼近、正则化和最优解”这个问题。它把“函数”也当作几何对象来处理，于是长度、相似性与复杂度都可以被严格定义。

#### 表示定理（Representer Theorem）

**定理**：在 RKHS 中，正则化经验风险最小化问题：

$$\min_{f \in \mathcal{H}} \sum_{i=1}^{n} L(y_i, f(x_i)) + \lambda \|f\|_{\mathcal{H}}^2$$

的解具有形式：

$$f^*(x) = \sum_{i=1}^{n} \alpha_i k(x, x_i)$$

这意味着：**无穷维优化问题的解可以用有限个训练样本的核函数线性组合表示**。这是核方法计算可行性的理论基石。

它回答的核心问题是：虽然特征空间可能是无穷维的，但真正需要优化的自由度仍然可以压缩回训练样本的有限线性组合，因此“理论上可写”才真正变成“工程上可算”。

#### 直觉理解

核方法的哲学是：**在低维空间中线性不可分的数据，映射到足够高维的空间后就变得线性可分**。RBF 核将数据映射到无穷维空间，在那里几乎任何数据集都可以被超平面分开。

一个经典的例子：XOR 问题在二维平面上线性不可分，但映射到三维空间 $(x_1, x_2, x_1 x_2)$ 后就可以用一个平面分开。

#### 一个极小例子：XOR 为什么需要核技巧

设四个点分别为 $(0,0)$、$(0,1)$、$(1,0)$、$(1,1)$，其中异或标签规定“恰有一个坐标为 1 时标签为正类”。在原二维空间里，正类点位于对角线两侧，任何直线都无法把它们与负类同时分开。

如果改用映射

$$
\phi(x_1, x_2) = (x_1, x_2, x_1x_2)
$$

那么 $(1,1)$ 会被抬到第三维，而另外三个点的第三维不同，问题就可能被一个平面分开。这个例子说明：核方法并不是“神奇地变复杂”，而是在高维函数空间里把原本难分的数据重新组织成更简单的几何形状。

#### AI 中的角色

- **SVM**：核方法 + 最大间隔 = 2000 年代最强的分类器
- **高斯过程**：核函数定义了函数的先验分布，实现贝叶斯非参数学习
- **核 PCA**：在高维特征空间中做主成分分析，捕捉非线性结构
- **MMD（最大均值差异）**：用核函数度量分布之间的距离，应用于 GAN 训练和域适应

---

### 3.5 神经正切核（Neural Tangent Kernel, NTK）

#### 背景与动机

深度学习的理论困境：

1. 损失函数高度非凸，为什么梯度下降能找到好的解？
2. 过参数化网络为什么不过拟合？
3. 不同的随机初始化为什么收敛到相似的性能？

NTK 理论为这些问题提供了统一的解释框架。

#### 数学定义

考虑参数为 $\theta$ 的神经网络 $f(x; \theta)$。**神经正切核**定义为：

$$\Theta(x, x') = \left\langle \frac{\partial f(x; \theta)}{\partial \theta}, \frac{\partial f(x'; \theta)}{\partial \theta} \right\rangle$$

即网络输出关于参数的梯度的内积。

#### 核心结论

**定理（Jacot et al., 2018）**：当网络宽度趋于无穷时：

1. **NTK 在初始化时收敛到确定性核**：$\Theta_0(x, x') \to \Theta^*(x, x')$
2. **NTK 在训练过程中保持不变**：$\Theta_t \approx \Theta_0$（lazy training regime）
3. **梯度下降等价于核回归**：网络的训练动态等价于在 NTK 对应的 RKHS 中做核回归

具体地，在连续时间梯度下降下，网络输出的演化满足：

$$\frac{d f_t(x)}{dt} = -\Theta^*(x, X)(f_t(X) - Y)$$

其中 $X, Y$ 是训练数据。这是一个线性 ODE，解为：

$$f_t(x) = f_0(x) + \Theta(x, X)\Theta(X, X)^{-1}(I - e^{-\Theta(X,X)t})(Y - f_0(X))$$

当 $t \to \infty$ 时，收敛到核回归的解。

#### 直觉理解

NTK 理论揭示了一个深刻的联系：**无限宽的神经网络 = 核方法**。训练神经网络就是在一个由网络架构决定的 RKHS 中做函数逼近。

但这也暴露了 NTK 理论的局限：实际的深度学习成功恰恰依赖于**有限宽度**下 NTK 的变化（特征学习），而非 lazy training。

#### AI 中的角色

- 为理解神经网络的**泛化能力**提供了理论工具
- 解释了**过参数化**网络为什么能泛化：NTK 的隐式正则化
- 连接了**核方法**和**深度学习**两大范式
- 推动了**无限宽度极限**下的理论研究（均场理论、张量程序等）

需要补一句边界说明：NTK 最擅长解释的是“无限宽、接近线性化”的训练机制，它是理解可学习性的重要窗口，但并不等同于有限宽度深度网络全部优势的最终解释。

---

## 4. 对 AI 的核心贡献

### 4.1 理论基础层

| 贡献 | 具体内容 | 影响 |
|------|----------|------|
| 存在性保证 | 万能逼近定理 | 神经网络的理论合法性 |
| 效率分析 | Barron 定理、深度分离 | 指导架构设计 |
| 正则化理论 | RKHS 范数、表示定理 | 防止过拟合的数学基础 |
| 优化理论 | NTK、损失景观分析 | 理解训练动态 |

### 4.2 算法层

**核方法家族**：

- **支持向量机（SVM）**：核技巧 + 最大间隔原理。在深度学习崛起前，SVM 是图像分类、文本分类的主流方法。其成功证明了逼近论思想的实用价值。
- **高斯过程（GP）**：核函数定义函数先验，贝叶斯推断给出后验。GP 提供了不确定性估计，在贝叶斯优化（超参数调优）中不可替代。
- **核岭回归 / 核 PCA**：将线性方法通过核技巧推广到非线性场景，是"核化"思想的典型应用。

**逼近论启发的网络设计**：

- **径向基函数（RBF）网络**：直接使用高斯核作为隐层激活函数，每个神经元对应一个局部逼近基函数。
- **残差网络（ResNet）**：深度分离定理证明了深度的价值，残差连接解决了深层网络的优化困难。
- **混合专家模型（MoE）**：将输入空间分区，每个区域用专门的子网络逼近，体现了分片逼近的思想。

### 4.3 理解层

逼近论为理解深度学习的以下现象提供了框架：

- **双下降现象（Double Descent）**：传统偏差-方差权衡在过参数化区域失效。逼近论视角：过参数化网络在更大的函数空间中搜索，RKHS 范数提供隐式正则化。
- **彩票假说（Lottery Ticket Hypothesis）**：大网络中存在小的子网络能达到相同性能。逼近论视角：逼近所需的有效参数远少于网络总参数。
- **神经缩放定律（Neural Scaling Laws）**：模型性能随参数量、数据量幂律增长。逼近论视角：逼近误差的衰减速率由目标函数的光滑性决定。

### 4.4 从逼近论到大模型时代

大语言模型（LLM）的成功可以从逼近论角度理解：

1. **Transformer 的逼近能力**：Yun et al. (2020) 证明了 Transformer 是序列到序列函数的万能逼近器。
2. **上下文学习（In-Context Learning）**：Garg et al. (2022) 表明 Transformer 可以在上下文中隐式实现核回归等学习算法。
3. **涌现能力（Emergent Abilities）**：可能与逼近误差的相变行为有关——当模型规模超过某个阈值时，逼近精度发生质的飞跃。

---

## 5. 前沿与开放问题

### 5.1 逼近理论的未解问题

1. **最优逼近速率**：对于给定的函数类和网络架构，最优逼近速率是什么？目前只有部分函数类（如 Barron 类、Sobolev 类）有精确结果。

2. **深度的精确收益**：深度为 L 的网络相比深度为 L-1 的网络，逼近能力提升多少？Telgarsky 的结果给出了指数级分离，但对实际网络架构的指导仍不够精确。

3. **逼近与泛化的关系**：逼近误差小不等于泛化误差小。如何在逼近能力和泛化能力之间取得最优平衡？

4. **自适应逼近**：神经网络能否自动适应目标函数的局部光滑性？即在函数变化剧烈的区域分配更多参数？

### 5.2 核方法的复兴

1. **NTK 之外**：有限宽度网络的特征学习如何超越 NTK 的 lazy training？均场理论和张量程序提供了部分答案。

2. **核方法的可扩展性**：经典核方法的计算复杂度为立方级（n 为样本数）。随机特征、Nyström 近似等方法将其降低到 O(n·m)（m 为特征数），但与深度学习的可扩展性仍有差距。

3. **深度核学习**：将深度学习的特征提取能力与核方法的理论保证结合。例如，用深度网络学习核函数，或在深度网络的中间层应用核方法。

### 5.3 与其他前沿方向的交叉

- **逼近论 × 因果推断**：因果函数的逼近是否比一般函数更容易？
- **逼近论 × 隐私**：差分隐私约束下的最优逼近速率是什么？
- **逼近论 × 量子计算**：量子神经网络的逼近能力是否超越经典网络？

---

## 6. 推荐阅读与参考文献

### 经典文献

- Weierstrass, K. (1885). Über die analytische Darstellbarkeit sogenannter willkürlicher Functionen einer reellen Veränderlichen. *Sitzungsberichte der Königlich Preußischen Akademie der Wissenschaften zu Berlin*.
- Stone, M. H. (1937). Applications of the theory of Boolean rings to general topology. *Transactions of the American Mathematical Society*, 41(3), 375-481.
- Kolmogorov, A. N. (1957). On the representation of continuous functions of many variables by superposition of continuous functions of one variable and addition. *Doklady Akademii Nauk SSSR*, 114, 953-956.
- Aronszajn, N. (1950). Theory of reproducing kernels. *Transactions of the American Mathematical Society*, 68(3), 337-404.

### 关键论文：万能逼近定理

- Cybenko, G. (1989). Approximation by superpositions of a sigmoidal function. *Mathematics of Control, Signals and Systems*, 2(4), 303-314.
- Hornik, K., Stinchcombe, M., & White, H. (1989). Multilayer feedforward networks are universal approximators. *Neural Networks*, 2(5), 359-366.
- Barron, A. R. (1993). Universal approximation bounds for superpositions of a sigmoidal function. *IEEE Transactions on Information Theory*, 39(3), 930-945.
- Lu, Z., et al. (2017). The expressive power of neural networks: A view from the width. *NeurIPS*.

### 核方法与 RKHS

- Vapnik, V. N. (1995). *The Nature of Statistical Learning Theory*. Springer.
- Schölkopf, B., & Smola, A. J. (2002). *Learning with Kernels*. MIT Press.
- Rahimi, A., & Recht, B. (2007). Random features for large-scale kernel machines. *NeurIPS*.

### 神经正切核

- Jacot, A., Gabriel, F., & Hongler, C. (2018). Neural tangent kernel: Convergence and generalization in neural networks. *NeurIPS*.
- Arora, S., et al. (2019). On exact computation with an infinitely wide neural net. *NeurIPS*.
- Lee, J., et al. (2019). Wide neural networks of any depth evolve as linear models under gradient descent. *NeurIPS*.

### 深度与逼近

- Telgarsky, M. (2016). Benefits of depth in neural networks. *COLT*.
- Eldan, R., & Shamir, O. (2016). The power of depth for feedforward neural networks. *COLT*.
- Yun, C., et al. (2020). Are transformers universal approximators of sequence-to-sequence functions? *ICLR*.

### 教材与综述

- DeVore, R. A. (1998). Nonlinear approximation. *Acta Numerica*, 7, 51-150.
- Pinkus, A. (1999). *Approximation Theory of the MLP Model in Neural Networks*. Cambridge University Press.
- Schölkopf, B., Herbrich, R. & Smola, A. J. (2001). A generalized representer theorem. *Computational Learning Theory*, 416-426.
- Poggio, T., Mhaskar, H., Rosasco, L., Miranda, B. & Liao, Q. (2017). Why and when can deep-but not shallow-networks avoid the curse of dimensionality. *International Journal of Automation and Computing*, 14, 503-519.
- Gühring, I., Raslan, M., & Kutyniok, G. (2020). Expressivity of deep neural networks. *arXiv:2007.04759*.
- Mhaskar, H. N. & Poggio, T. (2016). Deep vs. shallow networks: An approximation theory perspective. *Analysis and Applications*, 14(6), 829-848.

## 7. 本篇在全书中的位置

这一篇回答的是“模型为什么有可能表达复杂智能行为”，因此它与线性代数、微积分、概率论形成清晰分工：

- [[02-linear-algebra|线性代数]] 提供表示与计算语言。
- [[03-calculus-and-optimization|微积分与优化理论]] 解释如何把参数真正训练出来。
- [[01-probability-and-statistics|概率论与数理统计]] 解释如何在不确定性下建模、估计与评估。
- 本篇则聚焦“表达能力、逼近效率、可学习性与泛化”这些更偏理论边界的问题。

如果说线性代数告诉我们“模型如何写成矩阵与向量”，优化告诉我们“模型如何学”，那么逼近论与泛函分析告诉我们的就是“为什么这类模型原则上值得学，以及哪些结论只能算存在性结论，哪些才真正接近现代训练与泛化问题”。

---

> **总结**：逼近论是连接纯数学与深度学习的关键桥梁。从 Weierstrass 的多项式逼近到 Cybenko 的万能逼近定理，从 Aronszajn 的 RKHS 到 Jacot 的 NTK，逼近论不断为 AI 提供新的理论工具和设计灵感。在大模型时代，理解逼近的本质——用有限的参数捕捉无限的复杂性——比以往任何时候都更加重要。
