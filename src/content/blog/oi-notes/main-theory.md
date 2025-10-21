---
title: 主定理
publishDate: 2022-09-23
description: 'CSP初赛复习'
tags:
  - oi_note

language: '中文'
---

## 主定理

将一个规模为 $n$ 的问题，分治为 $a$ 个 $\lfloor\dfrac{n}{b}\rfloor$ 的子问题，每次带来的额外计算为 $\mathcal{O}(n^d)$，可得到以下关系式：

$$
T(n)=aT(\lceil\dfrac{n}{b}\rceil)+\mathcal{O}(n^d)
$$

$$
(\text{for\,constants\,}a>0,b>1,d\ge0),\text{then}:
$$

$$
T(n) = \begin{cases}
\mathcal{O}{(n^d)}, & \quad d>\operatorname{log}_ba\\
\mathcal{O}{(n^d\operatorname{log}n)}, & \quad d=\operatorname{log}_ba\\
\mathcal{O}{(n^{\operatorname{log}_ba})}, & \quad d<\operatorname{log}_ba\\
\end{cases}
$$
