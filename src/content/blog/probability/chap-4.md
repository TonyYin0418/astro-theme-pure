---
title: '随机变量的数字特征'
publishDate: 2025-04-29
description: '武汉大学2025春季学期 概率论与数理统计 第四章 随机变量的数字特征'
tags:
  - whu
  - probability

language: '中文'
---

第四章：随机变量的数字特征

本章主要讨论描述随机变量某种特征的常数。

## 数学期望

设离散型随机变量 $X$ 的取值为 $x_1,x_2,\ldots,x_n$，对应的概率为 $p_1,p_2,\ldots,p_n$，则称随机变量 $X$ 的数学期望为：
$$
E(X) = \sum_{k=1}^{\infty}x_k \cdot p_k
$$
对于连续型随机变量，类似有：
$$
E(X)=\int_{-\infty}^{\infty}xf(x)\text{d}x
$$
简称期望。

**「随机变量的函数的期望」**

设 $X$ 是离散型随机变量，$Y=g(x)$ 是 $X$ 的函数，分布律满足 $P(X=x_k)=p_k$，则 $Y$ 的期望为：
$$
E(Y)=E[g(X)]=\sum_{k=1}^{\infty}g(x_k)\cdot p_k
$$
连续型类似有：
$$
E(Y)=E[g(X)]=\int_{-\infty}^{\infty}g(x)f(x)\text{d}x
$$
所以，**我们在求 $E(Y)$ 时，不必算出 Y 的分布律或概率密度，只需使用 X 的分布律或概率密度。**

**「性质」**

1. 若 $C$ 是常数，$E(C)=C$.
2. 若 $C$ 是常数，$E(CX)=CE(X)$.
3. $E(X+Y)=E(X)+E(Y)$.
4. **若 $X, Y$ 相互独立**，$E(XY)=E(X)E(Y)$.

## 方差

**「定义」**

设 $X$ 是随机变量，**方差**：
$$
D(X)=\text{Var}(X)=E\{[X-E(X)]^2\}
$$
而**标准差为**
$$
\sigma_X = \sqrt{D(X)} = \sqrt{\mathrm{Var}(X)}
$$
把期望的公式代入，对于离散型随机变量：
$$
D(X)=\sum_{k=1}^{\infty}[x_k-E(X)]^2p_k
$$
连续型随机变量：
$$
D(X)=\int_{-\infty}^{\infty}[x-E(X)]^2f(x)\text{d}x
$$
**「期望和方差的关系」**
$$
D(X)=E(X^2)-[E(X)]^2
$$
**「切比雪夫不等式」**

设随机变量 $X$ 具有数学期望 $E(X)=\mu$ 和方差 $D(X)=\sigma^2$，则对任意正数 $\epsilon$，都有：
$$
P\{|X-\mu|\geq \epsilon\}\leq \frac{\sigma^2}{\epsilon^2}
$$
用于在不知道分布，只知道方差和期望时，估计这种概率的界限，但是**精度比较粗糙**。

这个公式说明：偏离均值较大的概率不会太高。

**「性质」**

1. 若 $C$ 是常数，$D(C)=0$.

2. 若 $C$ 是常数，$D(CX)=C^2\cdot D(X)$.

3. $D(X+Y)=D(X)+D(Y)+2E\{[X-E(X)][Y-E(Y)]\}$，

    当 $X, Y$ 独立时，有 $D(X+Y)=D(X)+D(Y)$

4. $D(X)=0 \Longleftrightarrow P\{X=E(X)\}=1$.

## 协方差和相关系数

**「定义」**

设 $X$ 和 $Y$ 是两个随机变量，**协方差**：
$$
\text{Cov}(X, Y)=E\{[X-E(X)][Y-E(Y)]\}
$$
而**相关系数为**
$$
\rho_{XY}=\frac{\text{Cov}(X, Y)}{\sqrt{D(X)}\sqrt{D(Y)}}
$$
$\rho_{XY}$ 衡量 $X$ 与 $Y$ 的线性相关程度，范围为 $[-1, 1]$。

**「性质」**

**协方差**描述的是两个变量是否**同向或反向变化**，但数值大小依赖变量的单位。

1. $\text{Cov}(X, Y)=\text{Cov}(Y, X)$.
2. $\text{Cov}(aX, bY)=ab\cdot \text{Cov}(X, Y)$.
3. $\text{Cov}(X+Y, Z)=\text{Cov}(X, Z)+\text{Cov}(Y, Z)$.

**相关系数**是在协方差基础上进行**标准化**，反映两个变量之间的**线性关系强弱和方向**，数值范围在 -1 到 1 之间。

1. $|\rho_{XY}|\leq 1$.
2. $|\rho_{XY}=1|\Longleftrightarrow$ 存在常数 $a, b$ 使 $P\{Y=a+bX\}=1$.
3. 当 $\rho_{XY}=0$ 时，$X$ 和 $Y$ **不相关**，但不一定独立。

**「与二维正态分布」**

从正态密度函数出发，二维正态分布的密度为：
$$
f(x, y) = \frac{1}{2\pi \sigma_X \sigma_Y \sqrt{1-\rho^2}} \exp\left( -\frac{1}{2(1-\rho^2)} Q(x, y) \right)
$$
其中 $Q(x, y)$ 是一个二次型：
$$
Q(x, y) = \left( \frac{(x - \mu_X)^2}{\sigma_X^2} - 2\rho \frac{(x - \mu_X)(y - \mu_Y)}{\sigma_X \sigma_Y} + \frac{(y - \mu_Y)^2}{\sigma_Y^2} \right)
$$
这里面的**常数 $\rho$ 就是相关系数**。

## 常见分布

离散型分布：

| 分布名称   | 参数                       | 分布律                                               | 期望 $E[X]$   | 方差 $Var(X)$       | 记号                        |
| ---------- | -------------------------- | ---------------------------------------------------- | ------------- | ------------------- | --------------------------- |
| 伯努利分布 | $p$    | $P(X = 1) = p$<br>$P(X = 0) = 1 - p$ | $p$           | $p(1 - p)$          | $\mathrm{Bern}(p)$          |
| 二项分布   | $n$，$p$ | $P(X = k) = \binom{n}{k} p^k (1 - p)^{n - k}$        | $np$          | $np(1 - p)$         | $\mathrm{Bin}(n, p)$        |
| 几何分布   | $p$               | $P(X = k) = (1 - p)^{k - 1} p$ | $\dfrac{1}{p}$ | $\dfrac{1 - p}{p^2}$ | $\mathrm{Geom}(p)$          |
| 泊松分布   | $\lambda > 0$   | $P(X = k) = \dfrac{\lambda^k e^{-\lambda}}{k!}$      | $\lambda$     | $\lambda$           | $\mathrm{Poisson}(\lambda)$ |
| 超几何分布   | $N,M,n$       | $P(X = k) = \dfrac{\binom{M}{k} \binom{N - M}{n - k}}{\binom{N}{n}}$                                      | $n \dfrac{M}{N}$     | $n\dfrac{M}{N} \cdot (1-\dfrac{M}{N}) \cdot \dfrac{N - n}{N - 1}$ | $\mathrm{H}(N, M, n)$     |

连续型分布：

| 分布名称 | 参数            | 概率密度函数                                                 | 期望 $E[X]$         | 方差 $Var(X)$          | 记号                         |
| -------- | --------------- | ------------------------------------------------------------ | ------------------- | ---------------------- | ---------------------------- |
| 均匀分布 | $[a, b]$        | $f(x) = \frac{1}{b - a},\quad x \in [a, b]$                  | $\frac{a + b}{2}$   | $\frac{(b - a)^2}{12}$ | $\mathrm{U}(a, b)$           |
| 指数分布 | $\lambda > 0$   | $f(x) = \lambda e^{-\lambda x},\quad x \ge 0$                | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^2}$  | $\mathrm{Exp}(\lambda)$      |
| 正态分布 | $\mu, \sigma^2$ | $f(x) = \frac{1}{\sqrt{2\pi \sigma^2}} \exp\left( -\frac{(x - \mu)^2}{2\sigma^2} \right)$ | $\mu$               | $\sigma^2$             | $\mathcal{N}(\mu, \sigma^2)$ |

## 例题

若 $Y = aX + b$，则：
$$
E(Y) = aE(X) + b,\quad D(Y) = a^2D(X)
$$
