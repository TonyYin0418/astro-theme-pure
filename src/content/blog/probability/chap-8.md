---
title: '假设检验'
publishDate: 2025-06-03
description: '武汉大学2025春季学期 概率论与数理统计 第八章 假设检验'
tags:
  - whu
  - probability

language: '中文'

---

第八章：假设检验。

在参数估计后，这是统计推断类问题的另一部分。

## 假设检验

对总体分布中**某些未知参数**作某种假设，然后由抽取的样本**构造合适的统计量**，对假设进行判断真伪，称为假设检验。

提出的**原假设**记为 $H_0$，与之相反的**备择假设**记为 $H_1$.

**「基本思想」**

类似于反证法。很重要的思想是：

**由于小概率事件在一次试验中几乎不可能发生，所以如果发生了，则有理由认为假设不成立。**

**拒真错误**：$H_0$ 实际正确，拒绝 $H_0$；**取伪错误**：$H_0$ 实际错误，接受 $H_0$.

## 显著性检验

> 设总体 $X\sim N(\mu,\sigma^2)$，$\sigma$ 已知，$x_1,x_2\cdots,x_n$ 为来自 $X$ 的样本值，现对 $\mu$ 进行假设检验。
>
> 试说明在显著性水平 $\alpha=0.05$ 下，关于 $\mu$ 的检验问题 $H_0:\mu=\mu_0$，$H_1:\mu\neq \mu_0$ 的拒绝域为 $|z|=|\dfrac{\bar x-\mu_0}{\sigma/\sqrt{n}}|>z_{\alpha/2}$.

这里**显著性水平** $\alpha$ 表示的是：**拒真错误的概率**，是小概率事件。

先认为 $H_0$ 是正确的，则 $X\sim N(\mu_0,\sigma^2)$，有：
$$
\bar X\sim N(\mu_0, \frac{\sigma^2}{n})\quad \Rightarrow \frac{\bar X-\mu_0}{\sigma/\sqrt{n}}\sim N(0,1)
$$
然后结合正态分布的图像，可知拒绝域为两侧：
$$
p\left\{\left|\frac{\bar x-\mu_0}{\sigma/\sqrt{n}}\right|>z_{\alpha/2}\right\}=\alpha \quad\Rightarrow|z|=|\frac{\bar x-\mu_0}{\sigma/\sqrt{n}}|>z_{\alpha/2}
$$
在实际问题中，把 $\bar x$ 等数值代入，就可以判断这个观察值是否在小概率区间（拒绝域），从而判断假设真伪。

## 正态总体的假设检验

| $H_0$                                                        | 枢轴量                                                       | $H_1$                                                        | 拒绝域                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| $$\begin{array}{}\mu\leq \mu_0 \\ \mu\geq \mu_0 \\ \mu=\mu_0\end{array}$$<br>$\sigma^2$已知 | $\displaystyle Z = \frac{\bar{X} - \mu}{\sigma/\sqrt{n}} \sim N(0,1)$ | $$\begin{array}{}\mu> \mu_0 \\ \mu< \mu_0 \\ \mu\neq\mu_0\end{array}$$ | $$\begin{array}{}z\geq z_\alpha \\ z\leq -z_{\alpha} \\ |z|\geq z_{\alpha/2}\end{array}$$ |
| $$\begin{array}{}\mu\leq \mu_0 \\ \mu\geq \mu_0 \\ \mu=\mu_0\end{array}$$<br/>$\sigma^2$未知 | $\displaystyle T = \frac{\bar{X} - \mu}{S/\sqrt{n}} \sim t(n-1)$ | $$\begin{array}{}\mu> \mu_0 \\ \mu< \mu_0 \\ \mu\neq\mu_0\end{array}$$ | $$\begin{array}{}t\geq t_\alpha(n-1) \\ t\leq -t_{\alpha}(n-1) \\ |t|\geq t_{\alpha/2}(n-1)\end{array}$$ |
| $$\begin{array}{}\sigma^2\leq \sigma^2_0 \\ \sigma^2\geq \sigma^2_0 \\ \sigma^2=\sigma^2_0\end{array}$$<br/>$\sigma^2$已知 | $\displaystyle \frac{(n-1)S^2}{\sigma^2}\sim \chi^2(n-1)$    | $$\begin{array}{}\sigma^2> \sigma^2_0 \\ \sigma^2< \sigma^2_0 \\ \sigma^2\neq\sigma^2_0\end{array}$$ | $$\begin{array}{}\chi^2\geq \chi^2_\alpha(n-1) \\ \chi^2\leq \chi^2_{1-\alpha}(n-1) \\ \chi^2\geq \chi^2_{\alpha/2}(n-1)\text{ OR } \chi^2\leq \chi^2_{1-\alpha/2}(n-1)\end{array}$$ |

