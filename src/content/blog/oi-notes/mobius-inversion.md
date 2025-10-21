---
title: '莫比乌斯反演 & 狄利克雷卷积'
publishDate: 2021-12-26
description: '莫比乌斯反演、狄利克雷卷积与数论函数。'
tags:
  - oi_note

language: '中文'
---

莫比乌斯反演 & 狄利克雷卷积

# P1829 [国家集训队]Crash的数字表格

$$
\begin{aligned}
\operatorname{Ans}
&=\sum_{i=1}^{n}\sum_{j=1}^{m}\operatorname{lcm}(i, j)\\
&=\sum_{i=1}^{n}\sum_{j=1}^{m}\frac{i\times j}{\operatorname{gcd}(i, j)}\\
&=\sum_{d=1}^{\min(n,m)}\sum_{i=1}^n\sum_{j=1}^m\frac{i\times j}{d} [\gcd(i,j)=d]\\
&=\sum_{d=1}^{\min(n,m)}\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}\frac{(i\cdot d)\times (j\cdot d)}{d} [\gcd(i,j)=1]\\
&=\sum_{d=1}^{\min(n,m)}d\cdot\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}i\times j[\gcd(i,j)=1]\\
&=\sum_{d=1}^{\min(n,m)}d\cdot\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}i\times j\sum_{x\mid \gcd(i, j)}\mu(x)\\
&=\sum_{d=1}^{\min(n,m)}d\sum_{x=1}^{\min(\lfloor\frac{n}{d}\rfloor, \lfloor\frac{m}{d}\rfloor)}\mu(x)\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}(i\times j)[x\mid i][x\mid j]\\
&=\sum_{d=1}^{\min(n,m)}d\sum_{x=1}^{\min(\lfloor\frac{n}{d}\rfloor, \lfloor\frac{m}{d}\rfloor)}\mu(x)\cdot x^2\sum_{i=1}^{\lfloor\frac{n}{dx}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{dx}\rfloor}(i\times j)
\end{aligned}
$$

前面两个 $\sum$ 只需预处理 $\mu(x)$ 的前缀和，最后两个 $\sum$ 可以用等差数列 $\mathcal{O}(1)$ 算出，于是答案可线性计算。
$$
\begin{aligned}
g(n,m) &= \sum_{i=1}^n\sum_{j=1}^mi\times j\\
&=\frac{n(n+1)}{2}\frac{m(m+1)}{2}
\end{aligned}
$$
整除分块，复杂度为 $\sum\limits_{i=1}^n \sqrt\frac{n}{i}$，计算可知，当 $n=10^7$ 时，左边式子的值 $<2\times 10^7$，可以通过。

注意取模、逆元。

```cpp
for(int d = 1; d <= min(n, m); d++) {
    int t = min(n / d, m / d), tmp = 0;
    for(int l = 1, r; l <= t; l = r + 1) {
        r = min((n / d) / ((n / d) / l), (m / d) / ((m / d) / l));
        (tmp += (mu[r] - mu[l - 1]) % mod * g(n / d / l, m / d / l) % mod) %= mod;
    }
    ans = (ans + tmp * d % mod) % mod;
}
```

# P3312 [SDOI2014]数表


先忽略 $a$ 的限制？

$$
\begin{aligned}
\operatorname{Ans}
&= \sum_{i=1}^{n}\sum_{j=1}^{m}\sum_{x=1}^{\min(n, m)}x[x\mid i][x\mid j]\\
&= \sum_{x=1}^{\min(n, m)}x\lfloor\frac{n}{x}\rfloor\lfloor\frac{m}{x}\rfloor\\
\end{aligned}
$$

这样虽然形式简单，但并不能继续进行…

换成下面的形式：
$$
\begin{aligned}
\operatorname{Ans}
&= \sum_{i=1}^{n}\sum_{j=1}^{m}\sigma(\gcd(i,j))\\
&= \sum_{d=1}^{n} \sum_{i=1}^{n}\sum_{j=1}^{m} \sigma(d)[\gcd(i,j)=d]\\
&= \sum_{d=1}^{\min(n,m)} \sigma(d) \sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}[\gcd(i,j)=1]\\
&= \sum_{d=1}^{\min(n,m)} \sigma(d) \sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}\sum_{x\mid \gcd(i,j)}\mu(x)\\
&= \sum_{d=1}^{\min(n,m)} \sigma(d) \sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}\sum_{x}^{\min(\lfloor\frac{n}{d}\rfloor, \lfloor\frac{m}{d}\rfloor)}\mu(x)[x\mid i][x\mid j]\\
&= \sum_{d=1}^{\min(n,m)} \sigma(d) \sum_{x}^{\min(\lfloor\frac{n}{d}\rfloor, \lfloor\frac{m}{d}\rfloor)}\mu(x) \sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}[x\mid i]\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}[x\mid j]\\
&= \sum_{d=1}^{\min(n,m)} \sigma(d) \sum_{x}^{\min(\lfloor\frac{n}{d}\rfloor, \lfloor\frac{m}{d}\rfloor)}\mu(x) \sum_{i=1}^{\lfloor\frac{n}{dx}\rfloor}1\sum_{j=1}^{\lfloor\frac{m}{dx}\rfloor}1\\
&= \sum_{d=1}^{\min(n,m)} \sigma(d) \sum_{x}^{\min(\lfloor\frac{n}{d}\rfloor, \lfloor\frac{m}{d}\rfloor)}\mu(x) \lfloor\frac{n}{dx}\rfloor\lfloor\frac{m}{dx}\rfloor\\
\end{aligned}
$$

记 $t=dx$，则可以更改枚举顺序，继续简化：
$$
\begin{aligned}
\operatorname{Ans}
&= \sum_{d=1}^{\min(n,m)} \sigma(d) \sum_{x}^{\min(\lfloor\frac{n}{d}\rfloor, \lfloor\frac{m}{d}\rfloor)}\mu(x) \lfloor\frac{n}{dx}\rfloor\lfloor\frac{m}{dx}\rfloor\\
&= \sum_{t=1}^{n} \lfloor\frac{n}{t}\rfloor\lfloor\frac{m}{t}\rfloor \sum_{d\mid t} \sigma(d) \mu(\frac{t}{d})
\end{aligned}
$$
观察推导过程，可知当 $\sigma(d)\leq a$ 时才会对答案有贡献。

设 $g(t)=\sum_{d\mid t} \sigma(d) \mu(\frac{t}{d})$.

以 $a$ 为关键字，从小到大枚举询问。$a$ 变大时，有一些 $g(t)$ 的值需要增加，可以直接枚举新加入的数的所有倍数，来找到所有被贡献的 $t$.

用树状数组维护 $g(t)$，时间复杂度为 $\mathcal{O}(q\sqrt n\log n+n\log^2 n)$. 


# P3455 [POI2007]ZAP-Queries

$$
\begin{aligned}
\operatorname{Ans}
&= \sum_{i=1}^{a}\sum_{j=1}^{b}[\gcd(i, j) = d]\\
&= \sum_{i=1}^{\lfloor\frac{a}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{b}{d}\rfloor} [\gcd(i, j) = 1]\\
&= \sum_{i=1}^{\lfloor\frac{a}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{b}{d}\rfloor}\sum_{d\mid \gcd(i, j)}\mu(d)\\
&= \sum_{i=1}^{\lfloor\frac{a}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{b}{d}\rfloor}\sum_{x=1}^{\min(\lfloor\frac{a}{d}\rfloor,\lfloor\frac{b}{d}\rfloor)} \mu(x)[d\mid i][d\mid j]\\
&= \sum_{x=1}^{\min(\lfloor\frac{a}{d}\rfloor, \lfloor\frac{b}{d}\rfloor)}\mu(x)\sum_{i=1}^{\lfloor\frac{a}{d}\rfloor}[d\mid i]\sum_{j=1}^{\lfloor\frac{b}{d}\rfloor}[d\mid j]\\
&= \sum_{x=1}^{\min(\lfloor\frac{a}{d}\rfloor, \lfloor\frac{b}{d}\rfloor)}\mu(x)\lfloor\frac{a}{xd}\rfloor\lfloor\frac{b}{xd}\rfloor
\end{aligned}
$$

# P3172 [CQOI2015]选数

$$
\begin{aligned}
\operatorname{Ans}
&= \sum_{a_1=L}^{h}\sum_{a_2=L}^{h}\cdots\sum_{a_n=L}^{h}[\gcd(a_1,a_2\cdots, a_n)=k]\\
&= \sum_{a_1=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}\sum_{a_2=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}\cdots\sum_{a_n=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}[\gcd(a_1,a_2\cdots a_n)=1]\\
&= \sum_{a_1=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}\sum_{a_2=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}\cdots\sum_{a_n=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}\sum_{d\mid \gcd(a_1,a_2\cdots a_n)}\mu (d)\\
&= \sum_{a_1=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}\sum_{a_2=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}\cdots\sum_{a_n=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}\cdot \sum_{d=1}^{\lfloor\frac{h}{k}\rfloor}\mu (d)[d\mid a_1][d\mid a_2]\cdots [d\mid a_n]\\
&= \sum_{d=1}^{\lfloor\frac{h}{k}\rfloor}\mu (d)\cdot \sum_{a_1=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}[d\mid a_1]\cdot \sum_{a_2=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}[d\mid a_2]\cdots\sum_{a_n=\lceil\frac{l}{k}\rceil}^{\lfloor\frac{h}{k}\rfloor}[d\mid a_n]\\
&= \sum_{d=1}^{\lfloor\frac{h}{k}\rfloor}\mu (d)\cdot \sum_{a_1=\lfloor\frac{l-1}{k}\rfloor+1}^{\lfloor\frac{h}{k}\rfloor}[d\mid a_1]\cdot \sum_{a_2=\lfloor\frac{l-1}{k}\rfloor+1}^{\lfloor\frac{h}{k}\rfloor}[d\mid a_2]\cdots\sum_{a_n=\lfloor\frac{l-1}{k}\rfloor+1}^{\lfloor\frac{h}{k}\rfloor}[d\mid a_n]\\
&= \sum_{d=1}^{\lfloor\frac{h}{k}\rfloor}\mu (d)\cdot (\lfloor\frac{h}{kd}\rfloor - \lfloor\frac{l-1}{kd}\rfloor)^n
\end{aligned}
$$

杜教筛、整除分块。

# P6810 「MCOI-02」Convex Hull 凸包

$$
\begin{aligned}
\operatorname{Ans}
&= 	\sum_{i=1}^{n}\sum_{j=1}^{m}\tau(i)\tau(j)\tau(\gcd(i, j))\\
&= 	\sum_{x=1}^{\min(n, m)}\tau(x)
    \sum_{i=1}^{n}\sum_{j=1}^{m}\tau(i)\tau(j)[\gcd(i, j)=x]\\
&= 	\sum_{x=1}^{\min(n, m)}\tau(x)
    \sum_{i=1}^{\lfloor\frac{n}{x}\rfloor}
    \sum_{j=1}^{\lfloor\frac{m}{x}\rfloor}\tau(xi)\tau(xj)[\gcd(i, j)=1]\\
&= 	\sum_{x=1}^{\min(n, m)}\tau(x)
    \sum_{i=1}^{\lfloor\frac{n}{x}\rfloor}
    \sum_{j=1}^{\lfloor\frac{m}{x}\rfloor}\tau(xi)\tau(xj)
    \sum_{d\mid \gcd(i,j)}\mu(d)\\
&= 	\sum_{x=1}^{\min(n, m)}\tau(x)
    \sum_{i=1}^{\lfloor\frac{n}{x}\rfloor}
    \sum_{j=1}^{\lfloor\frac{m}{x}\rfloor}\tau(xi)\tau(xj)
    \sum_{d=1}^{\lfloor\frac{\min(n, m)}{x}\rfloor}\mu(d)[d\mid i][d\mid j]\\
&= 	\sum_{x=1}^{\min(n, m)}\tau(x)
    \sum_{d=1}^{\lfloor\frac{\min(n, m)}{x}\rfloor}\mu(d)
    \sum_{i=1}^{\lfloor\frac{n}{x}\rfloor}[d\mid i]\tau(xi)
    \sum_{j=1}^{\lfloor\frac{m}{x}\rfloor}[d\mid j]\tau(xj)\\
&= 	\sum_{x=1}^{\min(n, m)}\tau(x)
    \sum_{d=1}^{\lfloor\frac{\min(n, m)}{x}\rfloor}\mu(d)
    \sum_{i=1}^{\lfloor\frac{n}{xd}\rfloor}\tau(xid)
    \sum_{j=1}^{\lfloor\frac{m}{xd}\rfloor}\tau(xjd)
%&=  \sum_{x=1}^{\min(n, m)}\tau^3(x)
%    \sum_{d=1}^{\lfloor\frac{\min(n, m)}{x}\rfloor}\mu(d)\tau^2(d)
%    \sum_{i=1}^{\lfloor\frac{n}{xd}\rfloor}\tau(i)
%    \sum_{j=1}^{\lfloor\frac{m}{xd}\rfloor}\tau(j)\\
\end{aligned}
$$

对于 $\sum\limits_{x=1}^{\min(n, m)}\tau(x)
	\sum\limits_{d=1}^{\lfloor\frac{\min(n, m)}{x}\rfloor}\mu(d)$，暴力循环枚举即可。复杂度用调和级数算，是 $\mathcal{O}(n\ln n)$.

对于 $\sum\limits_{i=1}^{\lfloor\frac{n}{xd}\rfloor}\tau(xid)
	\sum\limits_{j=1}^{\lfloor\frac{m}{xd}\rfloor}\tau(xjd)$，可以在预处理后 $\mathcal{O}(1)$ 计算。

先线性筛出所有的 $\tau(x)$，预处理出 $s(x,y)=\sum\limits_{i=1}^{y} \tau(xi)$.

若 $x$ 固定，则 $y\in[1, \lfloor\frac{\min(n, m)}{x}\rfloor]$，所以预处理的复杂度也是 $\mathcal{O}(n\ln n)$ 级别的。

这tm会被卡常。

所以有更简单的推导方法：
$$
\begin{aligned}
\operatorname{Ans}
&= \sum_{i=1}^{n}\sum_{j=1}^{m}\tau(i)\tau(j)\tau(\gcd(i,j))\\
&= \sum_{i=1}^{n}\sum_{j=1}^{m}\tau(i)\tau(j)\sum_{d\mid \gcd(i,j)}1\\
&= \sum_{d=1}^{n}\cdot \sum_{i=1}^{n}[d\mid i]\tau(i)\cdot\sum_{j=1}^{n}[d\mid j]\tau(j)\\
&= \sum_{d=1}^{n}\cdot \sum_{i=1}^{\lfloor\frac{n}{d}\rfloor} \tau(id)\cdot \sum_{j=1}^{\lfloor\frac{m}{d}\rfloor} \tau(jd)
\end{aligned}
$$
预处理出 $\sum\limits_{i=1}^{\lfloor\frac{n}{d}\rfloor} \tau(id)$ 即可。

# P3704 [SDOI2017]数字表格

$$
\begin{aligned}
\operatorname{Ans} &= \prod_{i=1}^n \prod_{j=1}^m f_{\gcd(i,j)}\\
&= \prod_{d=1}^{\min(n, m)} f_d ^{\sum_{i=1}^n \sum_{j=1}^m [\gcd(i,j)=d]}\\
\end{aligned}
$$

其中：
$$
\begin{aligned}
&= \sum_{i=1}^n \sum_{j=1}^m [\gcd(i,j)=d]\\
&= \sum_{x=1}^{\min(n/d,m/d)} \mu(x) \sum_{i=1}^{n/d} [x\mid i] \sum_{j=1}^{m/d} [x\mid j]\\
&= \sum_{x=1}^{\min(n/d,m/d)} \mu(x) (n/dx)(m/dx)\\
\end{aligned}
$$
继续：
$$
\begin{aligned}
\operatorname{Ans} 
&= \prod_{d=1}^{\min(n, m)} {f_d}^{\sum\limits_{i=1}^n \sum\limits _{j=1}^m [\gcd(i,j)=d]}\\
&= \prod_{d=1}^{\min(n, m)} {f_d}^{\sum\limits_{x=1}^{\min(n/d,m/d)} \mu(x) (n/dx)(m/dx)}\\
\end{aligned}
$$
常见套路，设 $t=dx$，则：
$$
\begin{aligned}
\operatorname{Ans} 
&= \prod_{d=1}^{\min(n, m)} {f_d}^{\sum\limits_{x=1}^{\min(n/d,m/d)} \mu(x) (n/dx)(m/dx)}\\
&= \prod_{t=1}^{\min(n, m)} \prod_{d\mid t} {f_d}^{\mu(t/d) (n/t)(m/t)}\\
&= \prod_{t=1}^{\min(n, m)} \prod_{d\mid t} {({f_d}^{\mu(t/d)})}^{(n/t)(m/t)}\\
\end{aligned}
$$
设 $g_t=\prod_{d\mid t} {f_d}^{\mu (t/d)}$，则：
$$
\begin{aligned}
\operatorname{Ans} 
&= \prod_{t=1}^{\min(n, m)} \prod_{d\mid t} {({f_d}^{\mu(t/d)})}^{(n/t)(m/t)}\\
&= \prod_{t=1}^{\min(n, m)} {g_t}^{(n/t)(m/t)}\\
\end{aligned}
$$
枚举每个 $d$，预处理出 $g_t$ 数组，复杂度为调和级数 $\frac{n}{1}+\frac{n}{2}+\cdots+\frac{n}{n}=n\ln n$.

多测，对每次询问，利用数论分块计算 $\operatorname{Ans}$，需要先算出 $g_t$ 的前缀积。

时间复杂度为 $\mathcal{O}(t\sqrt n+n\ln n)$.

**注意**：代码取模时，指数部分要 $\bmod (p-1)$ 而不是 $\bmod p$，由费马小定理知 $a^{p-1} \equiv a^0\pmod p$.

# P2257 YY的GCD

$$
\begin{aligned}
\operatorname{Ans}
&= \sum_{i=1}^n \sum_{j=1}^m \sum_{p=1}^{\min(n,m)} [\gcd(i,j)=p] ,p\in \mathrm{prime}\\
&= \sum_{p=1}^{\min(n,m)} \sum_{i=1}^{\lfloor\frac{n}{p}\rfloor} \sum_{j=1}^{\lfloor\frac{m}{p}\rfloor} [\gcd(i,j)=1] ,p\in \mathrm{prime}\\
&= \sum_{p=1}^{\min(n,m)} \sum_{i=1}^{\lfloor\frac{n}{p}\rfloor} \sum_{j=1}^{\lfloor\frac{m}{p}\rfloor} \sum_{x\mid \gcd(i,j)} \mu(x),p\in \mathrm{prime}\\
&= \sum_{p=1}^{\min(n,m)} \sum_{x=1}^{\min(\lfloor\frac{n}{p}\rfloor, \lfloor\frac{m}{p}\rfloor)}\mu(x)\lfloor\frac{n}{px}\rfloor\cdot \lfloor\frac{m}{px}\rfloor,p\in \mathrm{prime}\\
\end{aligned}
$$

设 $t=px$，
$$
\begin{aligned}
\operatorname{Ans}
&= \sum_{p=1}^{\min(n,m)} \sum_{x=1}^{\min(\lfloor\frac{n}{p}\rfloor, \lfloor\frac{m}{p}\rfloor)}\mu(x)\lfloor\frac{n}{px}\rfloor \lfloor\frac{m}{px}\rfloor,p\in \mathrm{prime}\\
&= \sum_{t=1}^{\min(n,m)} \lfloor\frac{n}{t}\rfloor \lfloor\frac{m}{t}\rfloor\sum_{x\mid t}\mu(x),\frac{t}{x}\in \mathrm{prime}\\
&= \sum_{t=1}^{\min(n,m)} \lfloor\frac{n}{t}\rfloor \lfloor\frac{m}{t}\rfloor\sum_{x\mid t}\mu(\frac{t}{x}),x\in \mathrm{prime}\\
\end{aligned}
$$

# P3911 最小公倍数之和

$$
\begin{aligned}
\operatorname{Ans} 
&= \sum_{i=1}^n \sum_{j=1}^n \operatorname{lcm}(A_i, A_j)\\
&= \sum_{i=1}^n \sum_{j=1}^n \frac{A_iA_j}{\gcd(A_i, A_j)}\\
&= \sum_{i=1}^n \sum_{j=1}^n \sum_{d=1}^m \frac{A_iA_j}{d}[\gcd(A_i, A_j)=d]\\
&= \sum_{i=1}^n \frac{A_i}{d} \sum_{j=1}^n \frac{A_j}{d} \sum_{d=1}^m \frac{1}{d}[\gcd(A_i, A_j)=1]\\
\end{aligned}
$$

上面是错的。

设 $c_i$ 为 $i$ 在 $A$ 中出现的次数，值域为 $[1,m]$，则：
$$
\begin{aligned}
\operatorname{Ans} 
&= \sum_{i=1}^n \sum_{j=1}^n \operatorname{lcm}(A_i, A_j)\\
&= \sum_{i=1}^m \sum_{j=1}^m \operatorname{lcm}(i, j)\cdot c_ic_j\\
&= \sum_{i=1}^m \sum_{j=1}^m \frac{i\cdot j}{\gcd(i,j)}\cdot c_ic_j\\
&= \sum_{i=1}^m i\cdot c_i \sum_{j=1}^m j\cdot c_j \sum_{d=1}^m \frac{1}{d}[\gcd(i,j)=d]\\
&= \sum_{d=1}^m \frac{1}{d} \sum_{i=1}^{m/d} {di}\cdot c_{di} \sum_{j=1}^{m/d} {dj}\cdot c_{dj} [\gcd(i,j)=1]\\
&= \sum_{d=1}^m \frac{1}{d} \sum_{i=1}^{m/d} {di}\cdot c_{di} \sum_{j=1}^{m/d} {dj}\cdot c_{dj}\sum_{x=1}^{m/d} \mu(x)\\
&= \sum_{d=1}^m \frac{1}{d} \sum_{x=1}^{m/d} \mu(x) \sum_{i=1}^{m/dx} {dxi}\cdot c_{dxi} \sum_{j=1}^{m/dx} {dxj}\cdot c_{dxj}\\
\end{aligned}
$$
预处理 $f(t)=\sum\limits_{i=1}^{m/t} ti\cdot c_{ti}$.

$$
\begin{aligned}
\operatorname{Ans}
&= \sum_{d=1}^m \frac{1}{d} \sum_{x=1}^{m/d} \mu(x) \sum_{i=1}^{m/dx} {dxi}\cdot c_{dxi} \sum_{j=1}^{m/dx} {dxj}\cdot c_{dxj}\\
&= \sum_{d=1}^m \frac{1}{d} \sum_{x=1}^{m/d} \mu(x) [f(dx)]^2\\
\end{aligned}
$$