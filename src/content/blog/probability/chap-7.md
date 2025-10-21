---
title: '参数估计'
publishDate: 2025-05-27
description: '武汉大学2025春季学期 概率论与数理统计 第七章 参数估计'
tags:
  - whu
  - probability

language: '中文'
---

第七章：参数估计。

这是统计推断类问题的一部分，另一部分是假设检验问题。

参数估计的目标是：**用样本数据，去估计总体的某个未知参数**，常见为：

- 总体均值 $\mu$，总体方差 $\sigma^2$，总体比例 $p$

## 点估计

设总体 $X$ 的分布函数**形式已知**，但是总体参数未知，要用一个样本来估计参数。

形式化地，设总体参数为 $\theta$，则用样本算出一个统计量 $ \hat{\theta}$ 来估计它。

### 矩估计

优点：推导简单直观，不依赖复杂的极大似然函数；

缺点：在某些分布下可能效率较低，不如最大似然估计（MLE）稳定。

**「步骤」**

1. 设总体的第 $k$ 阶原点矩为 $\mu'_k = E[X^k]$，这个式子会带有总体中的未知参数。
2. 样本的第 $k$ 阶样本矩为：
   $$
   m_k = \frac{1}{n} \sum_{i=1}^n X_i^k
   $$
3. 将前 $r$ 个样本矩 $m_1, m_2, \dots, m_r$ 分别等于总体矩 $\mu'_1, \mu'_2, \dots, \mu'_r$，解方程组得到 $r$ 个未知参数的估计量 $\hat{\theta}_1, \dots, \hat{\theta}_r$。
4. 最后选择合适的估计量。

**「举例：指数分布的矩估计」**

设总体 $X \sim \text{Exp}(\lambda)$，我们希望通过样本来估计参数 $\lambda$。

**步骤如下：**

第 1 步：写出总体矩（含参数）

指数分布的一阶原点矩为：
$$
\mu_1' = E[X] = \frac{1}{\lambda}
$$

第 2 步：计算样本矩（来自样本数据）

样本的一阶样本矩即为样本均值：
$$
m_1 = \bar{X} = \frac{1}{n} \sum_{i=1}^n X_i
$$

第 3 步：令**样本矩 = 总体矩**，解出参数估计量

令 $\bar{X} = \frac{1}{\lambda}$，可得：
$$
\hat{\lambda}_{\text{MM}} = \frac{1}{\bar{X}}
$$

> 设总体 $X$ 在 $[a,b]$ 上服从均匀分布，$a,b$ 未知，$X_1, X_2, \cdots,X_n$ 是来自 $X$ 的样本，求 $a, b$ 的矩估计量。

原点矩：
$$
\mu_1' = E[X] = \frac{a + b}{2}
$$

$$
\mu_2' = E[X^2] = D(X)+[E(X)]^2=\frac{(b-a)^2}{12}+\frac{(a+b)^2}{4}
$$

样本矩：
$$
A_1 = \bar{X} = \frac{1}{n} \sum_{i=1}^n X_i
$$

$$
A_2 = \frac{1}{n} \sum_{i=1}^n X_i^2
$$

联立等式：
$$
\begin{cases}
\frac{a + b}{2} = \bar{X} \\
\frac{a^2 + ab + b^2}{3} = A_2
\end{cases}
$$

整理得到最终结果：

$$
\hat{a} = \bar{X} - \sqrt{3(A_2 - \bar{X}^2)} \\
\hat{b} = \bar{X} + \sqrt{3(A_2 - \bar{X}^2)}
$$

### 极大似然估计

矩估计法求得的参数，是使得**样本矩等于总体矩的参数**。

作为对比，**极大似然估计**，是要找到一组参数，使得在该参数下，**此样本数据出现的概率最大**。

**「基本思想」**

是一种通过最大化样本数据在给定参数下的“出现概率”来估计参数的方法。

**核心思想：哪个参数最可能让“我看到的样本”出现？**

**「步骤」**

设样本 $X_1, X_2, \dots, X_n$ 独立同分布，密度函数为 $f(x;\theta)$，其中 $\theta$ 为未知参数：

1. 写出**似然函数**：
   $$
   L(\theta) = \prod_{i=1}^n f(X_i; \theta)
   $$

2. 通常取对数化简，得到**对数似然函数**：
   $$
   \ell(\theta) = \log L(\theta) = \sum_{i=1}^n \log f(X_i; \theta)
   $$

3. 对 $\ell(\theta)$ 对 $\theta$ 求导，解方程 $\ell'(\theta) = 0$，得到最大似然估计值 $\hat{\theta}_{\text{MLE}}$。

4. 检查导数符号或二阶导数，验证是否为极大值。

如果有多个参数，就改成分别求偏导，解**对数似然方程组**。

**「特点」**

- 适用范围广，尤其适合已知分布形式但参数未知的情形；
- 在样本量足够大时具有良好的统计性质（如一致性、渐近正态性、渐近有效性）；
- 往往比矩估计更有效，但求解过程需要更多微积分技巧。

**「例题」**

> 设总体的概率密度 $f(x)=\frac{1}{\theta}e^{-\frac{x-\mu}{\theta}},\; x>\mu$，其中 $\mu$ 和 $\theta$ 均未知，求其最大似然估计。

**解答：**

记样本为 $X_1, X_2, \dots, X_n$，由于样本独立，写出似然函数：
$$
L(\mu, \theta) = \prod_{i=1}^n \frac{1}{\theta} e^{-\frac{X_i - \mu}{\theta}} \cdot I(X_i > \mu)
= \frac{1}{\theta^n} e^{-\frac{1}{\theta} \sum (X_i - \mu)} \cdot I(\mu < X_{(1)})
$$
其中 $X_{(1)} = \min(X_1, \dots, X_n)$。

对数似然函数为：
$$
\ell(\mu, \theta) = -n \log \theta - \frac{1}{\theta} \sum (X_i - \mu), \quad \mu < X_{(1)}
$$

对 $\mu$ 求偏导：
$$
\frac{\partial \ell}{\partial \mu} = -\frac{n}{\theta}<0
$$
是单调递减函数，所以 $\ell$ 在 $\mu < X_{(1)}$ 区间上最大值取得于 $\mu = X_{(1)}$

固定 $\mu = X_{(1)}$，再对 $\theta$ 求偏导：

$$
\ell(\theta) = -n \log \theta - \frac{1}{\theta} \sum (X_i - X_{(1)})
$$

求导并令其为 0：
$$
\frac{d\ell}{d\theta} = -\frac{n}{\theta} + \frac{1}{\theta^2} \sum (X_i - X_{(1)}) = 0
$$

解得：
$$
\hat{\theta} = \frac{1}{n} \sum (X_i - X_{(1)})
$$

**最终结果：**
$$
\hat{\mu} = X_{(1)}, \quad
\hat{\theta} = \frac{1}{n} \sum_{i=1}^n (X_i - X_{(1)})
$$

---

## 估计量的评价

**「无偏性」**

- **估计量 $\hat{\theta}$ 的期望值等于参数 $\theta$，即 $E(\hat{\theta}) = \theta$。**
- 样本方差用 $\frac{1}{n-1}$ 是无偏的，而用 $\frac{1}{n}$ 则是有偏的。

**「一致性」**

- 当样本容量 $n \to \infty$ 时，估计量 $\hat{\theta}$ 收敛到参数 $\theta$。

**「有效性」**

  - 估计量 $\hat{\theta}$ 的**方差最小**，即在所有无偏估计量中，$\hat{\theta}$ 的方差最小。 

---

## 区间估计

在统计推断中，我们常常不仅想得到总体参数的一个**点估计**，还希望给出一个区间，这个区间能以某种置信程度覆盖真实的参数值，这被称为**区间估计**（Interval Estimation）。

**「置信区间」**

区间估计的结果称为**置信区间**，表示在重复抽样中有一定比例的此类区间能包含真实参数值。

总体 $X$ 有一个参数 $\theta$，样本 $X_1,X_2,\cdots,X_n$，满足：
$$
P\{\underline{\theta}(X_1,X_2,\cdots,X_n)<\theta<\overline{\theta}(X_1,X_2,\cdots,X_n)\}\geq 1-\alpha
$$
则随机区间 $(\underline{\theta}, \overline{\theta})$ 称为 $\theta$ 的 $1-\alpha$ 置信区间，上面的大于等于一般只看等号就行了。

定义 $(\underline{\theta}, \overline{\theta})$ 是**置信区间**，$1-\alpha$ 是**置信水平**，$\underline\theta$ 是**置信下限**，$\overline\theta$ 是**置信上限**。

**置信区间的含义：**每一次试验得到 $x_1,x_2,\cdots,x_n$，对应一个置信区间。反复抽样多次，在这么多次试验中，有 $1-\alpha$ 的比例的置信区间包含了真实的参数 $\theta$。

**「举例：正态总体 $\mu$ 的置信区间」**

> 设 $X \sim N(\mu, \sigma^2)$，样本均值为 $\bar{X}$，方差 $\sigma^2$ 已知，求 $\mu$ 的置信度为 $1-\alpha$ 的置信区间。

由题设，根据中心极限定理，样本均值 $\bar{X}$ 的分布为：
$$
\frac{\bar X-\mu}{\sigma/\sqrt{n}}\sim N(0,1)
$$
$$
\Rightarrow P\left\{-z_{\alpha/2} < \frac{\bar X - \mu}{\sigma/\sqrt{n}} < z_{\alpha/2}\right\} = 1 - \alpha
$$

$$
\Rightarrow P\left\{\bar X - z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}} < \mu < \bar X + z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}}\right\} = 1 - \alpha
$$

进而可得：
$$
\mu \in \left[ \bar{X} - z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}},\ \bar{X} + z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}} \right]
$$
其中，$z_{\pm \alpha/2}$ 代表标准正态分布的分位点，需要查表，但已知。

所以这个区间是仅取决于样本均值 $\bar{X}$ 和已知的总体方差 $\sigma^2$ 的。

**「枢轴量」**

一个统计量，不依赖于未知参数。用于构造置信区间。

## 正态总体的区间估计

| 待估参数   | 条件            | 枢轴量                                                       | 双侧 $1-\alpha$ 置信区间公式                                 |
| ---------- | --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| $\mu$      | $\sigma^2$ 已知 | $\displaystyle Z = \frac{\bar{X} - \mu}{\sigma/\sqrt{n}} \sim N(0,1)$ | $\displaystyle \left[ \bar{X} \pm z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}} \right]$ |
| $\mu$      | $\sigma^2$ 未知 | $\displaystyle T = \frac{\bar{X} - \mu}{S/\sqrt{n}} \sim t(n-1)$ | $\displaystyle \left[ \bar{X} \pm t_{\alpha/2}(n-1) \cdot \frac{s}{\sqrt{n}} \right]$ |
| $\sigma^2$ | $\mu$ 已知      | $\displaystyle \frac{1}{\sigma^2}\sum_{i=1}^n(X_i-\mu)^2 \sim \chi^2(n)$ | $\displaystyle \left[ \frac{\sum\limits_{i=1}^n(X_i-\mu)^2}{\chi^2_{\alpha/2}(n)},\ \frac{\sum\limits_{i=1}^n(X_i-\mu)^2}{\chi^2_{1-\alpha/2}(n)} \right]$ |
| $\sigma^2$ | $\mu$ 未知      | $\displaystyle \frac{(n-1)S^2}{\sigma^2} =\frac{1}{\sigma^2}\sum_{i=1}^n(X_i-\bar X)^2\sim \chi^2(n-1)$ | $\displaystyle \left[ \frac{(n-1)S^2}{\chi^2_{\alpha/2}(n-1)},\frac{(n-1)S^2}{\chi^2_{1-\alpha/2}(n-1)} \right]$ |

其中 $S^2$ 是样本方差 $\frac{1}{n-1}\sum\limits_{i=1}^n(X_i-\bar X)^2$，$t_{\alpha/2}(n-1)$ 是 $t$ 分布的分位点，$\chi^2_{\alpha/2}(n)$ 是 $\chi^2$ 分布的分位点。

有些题给定**误差**，求解样本量 $n$，比如 $\displaystyle \left[ \bar{X} \pm z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}} \right]$ 中，**误差**是 $z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}}$.

