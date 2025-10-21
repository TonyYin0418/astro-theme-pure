---
title: 四边形不等式优化
publishDate: 2021-02-26
description: '四边形不等式优化，有大部分结论的详细证明'

tags:
  - oi_note

language: '中文'
---

# 四边形不等式优化

## 定义

### 四边形不等式

#### 定义

设 $w(x, y)$ 是定义在整数集合上的二元函数。若对于定义域上的任意整数 $a\leq b\leq c\leq d$，都有 $w(a, d)+w(b, c)\geq w(a, c)+w(b, d)$ 成立，则称函数 $w$ 满足**四边形不等式**。

简单描述为：**交叉小于包含**。

#### 定理

四边形不等式还可以表述为：

设 $w(x, y)$ 是定义在整数集合上的二元函数。若对于定义域上的任意整数 $a<b$，都有 $w(a, b+1)+w(a+1, b)\geq w(a, b) + w(a + 1, b + 1)$ 成立，则称函数 $w$ 满足四边形不等式。

这算是另一种定义，可以证明与定义等价。

### 区间包含单调性

#### 定义

如果对于任意的 $l\leq l'\leq r'\leq r$，都有 $w(l', r')\leq w(l, r)$ 成立，那么称函数 $w$ 具有区间包含单调性。

## 用途

一般用于**区间 DP** 问题，转移方程一般形如：
$$
dp[i, j]=\left\{
\begin{array}{lr}
\min_{i\leq k<j}{\{dp[i, k]+dp[k+1, j]+w[i, j]\}}, &i<j\\
0, &i=j\\
\infty, &i>j
\end{array}
\right.
$$
转移方程的含义：

1. $dp[i, j]$ 表示从 $i$ 到 $j$ 的最小花费，题目一般需要求出 $dp[1, n]$.
2. $dp[i, k]+dp[k+1][j]$ 体现递推关系，$k$ 在 $i, j$ 之间滑动，$k$ 存在一个**最优值**，使得 $dp[i, j]$ 最大/最小。
3. $w[i, j]$ 的性质非常重要。$w[i, j]$ 是与题目有关的费用，如果它满足**四边形不等式**和**区间包含单调性**，那么在 DP 的时候，就能进行四边形不等式优化。

这类中的经典问题就是石子合并。

## 用法

只需要简单的优化操作，就能把上面的 $\mathcal{O}(n^3)$ 变成 $\mathcal{O}(n^2)$.

把循环 $i\leq k\leq j$ 改为 $s[i, j-1]\leq k\leq s[i+1, j]$，其中 $s[i, j]$ 记录从 $i$ 到 $j$ 的最优分割点。

在计算 $dp[i, j]$ 的最值时，可以得到区间 $i, j$ 的最优分割点 $k$，记录到 $s[i, j]$ 中即可。

<img src="https://pic.tonyyin.top/2021/02/25/956bbeeef1d93.png" style="zoom:100%;" />

也就是说优化后的状态转移方程为：
$$
dp[i, j]=\left\{
\begin{array}{lr}
\min_{s[i, j-1]\leq k\leq s_[i+1, j]}{\{dp[i, k]+dp[k+1, j]+w[i, j]\}}, &i<j\\
0, &i=j\\
\infty, &i>j
\end{array}
\right.
$$


```cpp
for(int i = 1; i <= n; i++) {
	dp[i][i] = 0;
	s[i][i] = i; // s[][] 的初始化
}
for(int len = 2; len <= n; len++) {
    for(int i = 1; i <= n - len + 1; i++) {
        int j = i + len - 1;
        for(int k = s[i][j - 1]; k <= s[i + 1][j]; k++) { //四边形不等式优化
            if(dp[i][j] > dp[i][k] + dp[k + 1][j] + w[i][j]) {
                dp[i][j] = dp[i][k] + dp[k + 1][j] + w[i][j];
                s[i][j] = k;
            }
        }
    }
}
```

**考场上**，对于能否使用四边形不等式优化的判断，一般情况下随机数据+$\mathcal{O}(n^3)$ DP之后，编程判断是否满足单调性就可以了，可以不用数学方法证明。

## 原理

### 引理1

#### 描述

对于上面那种形式的状态转移方程，如果 $w[i, j]$ 满足四边形不等式和区间包含单调性，那么 $dp[i, j]$ 也满足四边形不等式。

#### 证明

即证，对于 $a\leq b\leq c\leq d$，有 $dp[a, c]+dp[b, d]\leq dp[a, d]+dp[b, c]$.

不妨设 $dp[a, c]$ 的最优值在 $x$ 取到，$dp[b， d]$ 的最优值在 $y$ 取到，且 $x < y$，

于是可以得到：
$$
\begin{aligned}
dp[a, c]+dp[b, d] &= dp[a, x]+dp[x + 1, c] + w[a, c]+dp[b, y]+dp[y+1, d]+w[b, d]\\
\\
&\leq dp[a, x]+dp[x + 1, c]+w[a, d]+dp[b, y]+dp[y + 1, d]+w[b, c]\\
\\
&\leq dp[a, x]+dp[x + 1 , d]+w[a, d]+dp[b, y]+dp[y+1, c]+w[b, c]\\
\\
&=dp[a, d]+dp[b, c]
\end{aligned}
$$
其中第三行用到了结论 $dp[x + 1, c]+dp[y + 1, d]\leq dp[x + 1, d]+dp[y + 1, c]$，可以同理递归证明。

同理若 $y<x$ 命题也成立。

### 引理2

#### 描述

记 $s[i, j]=k$ 是 $dp[i, j]$ 取得最优值时的 $k$，如果 dp 满足四边形不等式，那么有 $s[i, j-1]\leq s[i, j]\leq s[i+1, j]$，即 $s[i, j-1]\leq k\leq s[i+1, j]$.

#### 证明

记 $f[k, i, j]=dp[i, k]+dp[k + 1, j] + w[i, j]$，

只需证所有不在 $[s[i, j-1], s[i+1, j]]$ 区间内的决策点肯定不是最优的。

那么决策点位置有两种：在区间左右两侧，不妨设在左侧。

也就是说，记 $x=s[i, j-1]$，决策点 $y\in(i, x)$.

此时若证明证 $f[x, i, j]\leq f[y, i, j]$，就能说明 $y\in(i, x)$ 不是最优的。

根据 $s$ 的定义，显然有 $f[x, i, j-1]\leq f[y, i, j-1]$，

故证明 $f[y, i, j-1]-f[x, i, j-1]\leq f[y, i, j]-f[x, i, j]$ 即可，

移项得到只需证 $f[y, i, j-1]+f[x, i, j]\leq f[y, i, j]+f[x, i, j-1]$，

根据 $f$ 的定义，这就等价于证明：
$$
\begin{aligned}
dp[i, y]+dp[y+1, j-1]+w[i, j-1]+dp[i, x]+dp[x+1, y]+w[i, j]\\
\leq dp[i, y]+dp[y + 1, j]+w[i, j]+dp[i, x]+dp[x + 1, j-1]+w[i, j-1]
\end{aligned}
$$
消去相同项，只需证：
$$
dp[y+1, j-1]+dp[x + 1, y]\leq dp[y+1, j] +dp[x + 1, j-1]
$$
又因为 $dp$ 满足四边形不等式，故上式成立。

对于决策点在区间右侧的情况同理可证。

### 定理——Knuth-Yao DP Speedup Theorem

#### 描述

如果 $w(i, j)$ 满足四边形不等式和区间包含单调性，则用 DP 计算 $dp$ 数组的时间复杂度是 $O(n^2)$ 的。

#### 证明

$$
\begin{aligned}
dp[i, j]=\left\{
\begin{array}{lr}
\min_{s[i, j-1]\leq k\leq s_[i+1, j]}{\{dp[i, k]+dp[k+1, j]+w[i, j]\}}, &i<j\\
0, &i=j\\
\infty, &i>j
\end{array}
\right.
\end{aligned}
$$

在求 $dp[i, j]$ 的时候，$s[i, j-1]$ 和 $s[i+1, j]$ 一定已经算出，这样缩小决策变量范围进行优化。
$$
\begin{align}
&\sum_{len=2}^n\sum_{i=1}^{n-len+1}(1+s[i+1, i+len-1]-s[i, i+len-2])\\
&=\sum_{len=2}^{n}
\left[
\sum_{i=1}^{n-len+1}(1) + \sum_{i=1}^{n-len+1}(s[i+1, i+len-1]) - 	\sum_{i=1}^{n-len+1}(s[i, i+len-2])
\right]\\
&=\sum_{len=2}^{n}
\left\{
(n-len+1)+s[n-len+2, n]-s[1, len-1]
\right\}\\
&\leq \sum_{len=2}^{n}(2n-len+1)
\end{align}
$$
所以复杂度为 $\mathcal{O}(n^2)$.