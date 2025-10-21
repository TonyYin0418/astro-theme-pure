---
title: '大数定律及中心极限定理'
publishDate: 2025-05-06
description: '武汉大学2025春季学期 概率论与数理统计 第五章 大数定律及中心极限定理'
tags:
  - whu
  - probability

language: '中文'
---

第五章：大数定律及中心极限定理

本章主要讨论大数定律及中心极限定理。

## 大数定律

大数定律是概率论中的一个重要定理，样本均值随着样本容量的增加而趋近于总体均值的性质。

**「弱大数定律」**

设 $X_1, X_2, \ldots, X_n$ 是独立同分布的随机变量，数学期望 $E(X_k)=\mu$，则对于任意 $\epsilon>0$，有：
$$
P\left(\left|\frac{1}{n}\sum_{k=1}^n X_k - \mu\right|>\epsilon\right) \to 0 \quad (n \to \infty)
$$

通俗的说，就是当样本容量 $n$ 趋近于无穷大时，样本均值 $\frac{1}{n}\sum_{k=1}^n X_k$ 会以概率收敛到总体均值 $\mu$。

**「伯努利大数定律」**

设 $f_A$ 是 $n$ 次独立重复试验中事件 $A$ 发生的频率，$p$ 是事件 $A$ 的概率，则对于任意 $\epsilon>0$，有：
$$
P\left(\left|\frac{f_A}{n} - p\right|<\epsilon\right) \to 1 \quad (n \to \infty)
$$

## 中心极限定理

大数定律关注的是：$\bar{X}_n$ 最终会接近 $\mu$，而中心极限定理要解决的是：$\bar{X}_n$ 附近是怎样波动的？

简单来说，结论为：**不管原始变量是什么分布，平均值的分布都会近似正态。**

预先定义：对于随机变量 $X$ 有均值 $\mu$ 和方差 $\sigma^2$，则**标准化变量** $Z=\dfrac{X-\mu}{\sigma}$.

**「独立同分布的中心极限定理」**

设随机变量 $X_1, X_2, \ldots, X_n$ 独立同分布，数学期望 $E(X_k)=\mu$，方差 $D(X_k)=\sigma^2$，

则当 $n \to \infty$ 时：标准化变量满足
$$
Z_n=\frac{\sqrt{n}(\bar{X}_n - \mu)}{\sigma}=\frac{\sum X_n-n\cdot \mu}{\sqrt{n}\cdot \sigma}\sim N(0, 1)
$$
或改写为：
$$
\bar{X}_n\sim N(\mu, \frac{\sigma^2}{n})
$$
也就是说，算术平均值 $\bar{X}_n$，在 $n$ 充分大时，服从均值为 $\mu$，方差为 $\frac{\sigma^2}{n}$ 的正态分布。

**「Lyapunov 定理」**

比我们前面学的“独立同分布”版本更**一般化**，适用于**不完全相同分布**的情形。

设 $X_1, X_2, \ldots, X_n$ 独立同分布，数学期望 $E(X_k)=\mu_k$，方差 $D(X_k)=\sigma_k^2$，总方差为 $s_n^2=\sum_{k=1}^n \sigma_k^2$，

若存在 $\delta>0$ 使得当 $n \to \infty$ 时：
$$
\lim_{n \to \infty} \frac{1}{s_n^{2 + \delta}} \sum_{i=1}^n E\left[ |X_i - \mu_i|^{2 + \delta} \right] \to 0
$$
则结论与独立同分布的中心极限定理相同：标准化变量满足

$$
Z_n=\frac{\sum\limits_{k=1}^{n}X_k-\sum\limits_{k=1}^{n}\mu_k}{s_n} \sim N(0, 1)
$$

## 例题

例题中常用的主要是第一个定理，独立同分布的中心极限定理。

> 计算器在进行加法时，将每个加数舍入最靠近它的整数，设所有舍入误差相互独立且在 $(-0.5,0.5)$ 上服从均匀分布。
>
> - 将 $1500$ 个数相加，问误差综合的绝对值超过 $15$ 的概率是多少？
> - 最多可有几个数相加使得误差总和的绝对值小于 $10$ 的概率不小于 $0.90$.

$X_k$ 表示第 $k$ 个数的误差，$X_k \sim U(-0.5, 0.5)$，则 $E(X_k)=0$，$D(X_k)=\frac{1}{12}$.

设 $X=\sum_{k=1}^{1500}X_k$，则根据中心极限法则，
$$
Z=\frac{X-1500\times 0}{\sqrt{1500 \cdot \frac{1}{12}}} \sim N(0, 1)
$$

所以概率：
$$
P(|X|>15)=1-P\left(\left|Z\right|\leq\frac{15}{\sqrt{1500 \cdot \frac{1}{12}}}\right)=1-\left[2\Phi(\frac{15}{\sqrt{1500 \cdot \frac{1}{12}}})-1\right]=0.1802
$$
