---
title: 斯特林数
publishDate: 2022-03-02
description: '斯特林数学习笔记，zyc讲的，没学完'

tags:
  - oi_note

language: '中文'
---


# 第二类斯特林数

## 定义

第二类斯特林数，用 $S(n,m)$ 或 $\begin{Bmatrix}n\\m\end{Bmatrix}$ 表示：将 $n$ 个有标号元素分为 $m$ 个 **非空** 无标号集合的方案数。

## 递推式

$$
\begin{Bmatrix}n\\m\end{Bmatrix}=\begin{Bmatrix}n-1\\m-1\end{Bmatrix} + m\times \begin{Bmatrix}n-1\\m\end{Bmatrix}
$$

## 通项公式
$$
\begin{Bmatrix}n\\m\end{Bmatrix}=\sum_{i=0}^{m} \frac{i ^ n(-1)^{m-i}}{i!(m-i)!}
$$

## 行
$n$ 固定，$m$ 变。

注意到通项公式很像卷积式，直接设：

$$
[n^i]g(n)=\dfrac{i^n}{i!}, [n^i]h(n)=\dfrac{(-1)^{i}}{i!}
$$

由于通项公式中 $i + (m-i) = m$，直接卷积得到 $f=g*h$.

```cpp
Poly A, B;
A.a.resize(n+1), B.a.resize(n+1);
for(int i = 0; i &lt;= n; i++) A.a[i] = power(i, n) * fac_inv[i] % mod;
for(int i = 0; i &lt;= n; i++) B.a[i] = (i&1) ? mod - fac_inv[i] : fac_inv[i];
A = A * B;
```

## 列
TODO.

# 第一类斯特林数

## 定义

第一类斯特林数，用 $s(n,m)$ 或 $\begin{bmatrix}n\\m\end{bmatrix}$ 表示：将 $n$ 个有标号元素分为 $m$ 个**非空**无标号圆排列的方案数。

## 递推式

$$
\begin{bmatrix}n\\k\end{bmatrix}=\begin{bmatrix}n-1\\k-1\end{bmatrix}+(n-1)\begin{bmatrix}n-1\\k\end{bmatrix}
$$

# 行

https://www.luogu.com.cn/blog/oylyer/solution-p5408

记：

$$
F_n(x)=x^{\overline n}=\prod_{i=0}^n (x+i)
$$

则可证：

$$
x^{\overline n}=\sum_{i=1}^n \begin{bmatrix}n\\i\end{bmatrix} x^i
$$

由于显然有 $x^{\overline{2n}}=x^{\overline n}(x+n)^{\overline n}$，考虑分治求解。

$$
\begin{aligned}
F_n(x+n) &=\sum_{i=1}^n \begin{bmatrix}n\\i\end{bmatrix} (x+n)^i\\
&=\sum_{i=1}^n\begin{bmatrix}n\\i\end{bmatrix}\sum_{j=0}^i \binom{i}{j}x^jn^{i-j}\\
&=\sum_{i=1}^n\begin{bmatrix}n\\i\end{bmatrix}\sum_{j=0}^i \frac{i!}{j!(i-j)!}x^jn^{i-j}\\
&=\sum_{j=0}^n x^j \frac{1}{j!} \sum_{i=j}^n\begin{bmatrix}n\\i\end{bmatrix} i!\frac{n^{i-j}}{(i-j)!}\\
\end{aligned}
$$

设 $A_i=\begin{bmatrix}n\\i\end{bmatrix} i!$，$B_i=\dfrac{n^i}{i!}$，则原式化为：

$$
\begin{aligned}
F_n(x+n)
&=\sum_{j=0}^i x^j \frac{1}{j!} \sum_{i=1}^n\begin{bmatrix}n\\i\end{bmatrix} i!\frac{n^{i-j}}{(i-j)!}\\
&=\sum_{j=0}^i x^j \frac{1}{j!} \sum_{i=1}^n A_iB_{i-j}\\
\end{aligned}
$$

要凑卷积的形式，把 $B$ 翻转得到 $B^{\prime}_{n-i}=B_i$，则：

$$
\begin{aligned}
F_n(x+n)
&=\sum_{j=0}^i x^j \frac{1}{j!} \sum_{i=1}^n A_iB_{i-j}\\
&=\sum_{j=0}^i x^j \frac{1}{j!} \sum_{i=1}^n A_i{B^{\prime}}_{n+j-i}\\
\end{aligned}
$$

这样 $A*{B^{\prime}}$ 之后，得到的 $n+j$ 次项系数左移 $n$ 位，再整体 $\times \frac{1}{j!}$，就是 $F_n(x+n)$ 的 $j$ 次项系数。

再把 $F_n(x)*F_n(x+n)$，即可得到 $F_{2n}(x)$。

注意分治过程中，$F_{2n+1}=F_n(x)*F_n(x+n)*(x+2n)$，遇到奇数就手动乘上去即可。

```cpp
void solve(int x) { //求f(x)
	//x的一次上升幂f(x)=x，零次项系数为0，一次项系数为1
	if(x == 1) { F.a.resize(2); F.a[1] = 1; return; }
	//递归，通过f(n)求出f(x)
	solve(x &gt;&gt; 1);
	int n = (x &gt;&gt; 1);
	//计算出A、B
	A.a.resize(n+1), B.a.resize(n+1);
	for(int i = 0; i &lt;= n; i++) A.a[i] = F.a[i] * fac[i] % mod;
	for(int i = 0, pw = 1; i &lt;= n; i++, pw = pw * n % mod) 
        B.a[i] = pw * fac_inv[i] % mod;
	reverse(B.a.begin(), B.a.begin() + n + 1);
	//A与B卷积，把第n+1~2n位向左平移，之后A的每项系数除以阶乘，F再卷上A
	A = A * B;
	for(int i = 0; i &lt;= n; i++) A.a[i] = A.a[i + n];
	for(int i = 0; i &lt;= n; i++) A.a[i] = A.a[i] * fac_inv[i] % mod;
	A.a.resize(n+1);
	F = A * F;
	//若x是奇数，则2*n=x-1，上升幂需要再卷上一个(x+2*n)，左移，每个系数都乘上2*n
	if(x & 1) {
		F.a.resize(2 * n + 2);
		for(int i = 2 * n + 1; i &gt;= 1; i--) 
            F.a[i] = (F.a[i - 1] + (x - 1) * F.a[i] % mod) % mod;
		F.a[0] = F.a[0] * (x - 1) % mod;
	}
}
```

## 列

TODO。