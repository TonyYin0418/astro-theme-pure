---
title: 数论基础
publishDate: 2022-03-08
description: '没写完。
带余除法、欧几里得算法、筛法、欧拉函数与欧拉定理、扩展欧几里得算法、乘法逆元、中国剩余定理、扩展中国剩余定理、卢卡斯定理、扩展卢卡斯定理'

tags:
  - oi_note

language: '中文'
---

# 带余除法

## 整除

设 $a,b$ 是整数，$a\not= 0$，如果存在 $q$ 是整数，使得 $b=aq$，那么有 $b$ 可被 $a$ 整除，记作 $a \mid b$，且称 $b$ 是 $a$ 的**倍数**，$a$ 是 $b$ 的**因数**。

## 性质

1. 若 $a \mid b$ 且 $b \mid c$，则 $\forall x,y$，有$a \mid xb+yc$.
2. 若 $a \mid b$ 且 $b \mid c$，则 $a \mid c$.
3. 设 $m\neq 0$，则 $a\mid b$，当且仅当 $ma\mid mb$.
4. 若 $a\mid b$ 且 $b\mid a$，则 $a=\pm b$.
5. 若 $a \mid b$ 且 $b\not= 0$，则 $ |a|\leq |b|$.

## 带余除法

设 $a,b$ 是正整数，且 $b\not= 0$，则存在唯一整数 $q, r$ 使得 $a = qb+r,0\leq r < |b|$.

这个式子叫做带余除法，记余数 $r=a\bmod b$。当 $r=0$ 时，$b$ 是 $a$ 的因数。

## 性质

$$
(a\pm b)\bmod p=(a\bmod p\pm b\bmod p)\bmod p
$$

$$
(a\times b)\bmod p=(a\bmod p\times b\bmod p)\bmod p
$$

## 最大公因数与最小公倍数

## 最大公因数

设 $a$ 和 $b$ 是两个非零整数，如果 $d \mid a$ 且 $d \mid b$，则称 $d$ 是 $a$ 与 $b$ 的**公因数**。

所有公因数中最大的被称为 $a,b$ 的**最大公因数**，记为 $\gcd(a,b)$.

特殊地，当 $\gcd(a,b)=1$时，称 $a,b$ **互质**。


## 最小公倍数

设 $a$ 和 $b$ 是两个非零整数，如果 $a \mid d$ 且 $a \mid d$，则称 $d$ 是 $a$ 与 $b$ 的**公倍数**。

所有公倍数中最小的被称为 $a,b$ 的**最小公倍数**，记做 $\operatorname{lcm}(a,b)$.

## 性质

1. 若 $a \mid m,b \mid m$，则$\gcd(a,b) \mid m$.

2. 若 $d \mid a,d \mid b$，则 $d \mid \operatorname{lcm}(a,b)$.
3. $\operatorname{lcm}(a,b)=\dfrac{ab}{\gcd(a,b)}$.
4. 设 $m,a,b$ 是正整数，则 $\operatorname{lcm}(ma, mb)=m\times \operatorname{lcm}(a, b)$.
5. 若 $m$ 是非零整数 $a_1,a_2,a_3,\cdot\cdot\cdot, a_n$ 的公倍数，则 $\operatorname{lcm}(a_1, a_2,\cdot\cdot\cdot\;, a_n) \mid m$.

# 欧几里得算法

欧几里得算法，又名**辗转相除法**。

有结论：
$$
\gcd(a,b)=\gcd(b,a\bmod b)
$$
设 $a=qb+r,(r<q)$，$d\mid a$ 且 $d\mid b$.

由于 $a=qb+r$，故 $r=a-qb$，

另一方面，由于 $d\mid a$ 且 $d\mid b$，有 $d\mid a-qb$.

于是得到 $d\mid r$，所以任意 $a,b$ 的公因数都是 $b,r$ 的公因数，$r<a$，所以 $\gcd(a,b)=\gcd(b,r)$ 得证。

时间复杂度为 $\mathcal{O}(\log n)$，下面是代码实现。

~~~cpp
int gcd(int a, int b){
	if(b == 0) return a;
    return gcd(b, a % b);
}
~~~

# 筛法

## 埃氏筛法

**问题：**给定 $n$，预处理出每个 $[1,n]$ 范围内的整数是否为质数的算法。

**思路**：从小到大枚举，若确定 $x$ 为质数，则将 $x, 2x, 3x, \cdots, kx$ 标记为合数。若遍历到 $x$ 时，其仍然未被标记为合数，则可确定 $x$ 为质数。

还有以下优化：

1. $j$ 的初值改为 $j=i\cdot i$，因为对于 $j=i\cdot k,(k < i)$，合数 $j$ 会被 $k$ 提前筛掉。
2. $i$ 枚举的范围为 $[2,\sqrt{n}]$，因为对于 $i>\sqrt{n}$，$i^2>n$，起不到任何贡献。

  ~~~cpp
  for(int i = 2; i <= n; i++) is_prime[i] = true;
  for(int i = 2; i * i <= n; i++) {
      if(is_prime[i])
  		for(int j = i * i; j <= n; j += i) is_prime[j] = false;
  }
  ~~~

经过简单优化的埃氏筛，可证明时间复杂度为 $\mathcal{O}(n\log \log n)$，相较于 $\mathcal{O}(n)$ 的线性筛，埃氏筛并不实用。

## 欧拉筛法

欧拉筛法，又称**线性筛**，顾名思义，其时间复杂度为 $\mathcal{O}(n)$。

**思路**：在埃氏筛法中，每个合数可能会被筛多次，若每个合数恰好被筛一次，时间复杂度就是线性了。**在线性筛中，每一个合数都是被最小的质因子筛掉**，不过这在常见的代码实现中并不显然。

```cpp
int flag[N + 10], pri[N + 10], cnt = 0;
inline void Euler() {
	flag[1] = 1;
	for(int i = 2; i <= N; i++) {
		if(!flag[i]) pri[++cnt] = i;
		for(int j = 1; i * pri[j] <= N && j <= cnt; j++) {
			flag[i * pri[j]] = 1;
			if(i % pri[j] == 0) break;
		}
	}
}
```

## 线性筛莫比乌斯函数

$$
\mu(n)=
\begin{cases}
1&n=1\\
0&n\text{ 含有平方因子}\\
(-1)^k&k\text{ 为 }n\text{ 的本质不同质因子个数}\ \end{cases}
$$

莫比乌斯函数是积性函数。

```cpp
int flag[N + 10], pri[N + 10], mu[N + 10], cnt;
mu[1] = 1;
for(int i = 2; i <= N; i++) {
    if(!flag[i]) pri[++cnt] = i, mu[i] = -1;
    for(int j = 1; j <= cnt && i * pri[j] <= N; j++) {
        flag[i * pri[j]] = 1;
        if(i % pri[j] == 0) break;
        else mu[i * pri[j]] = -mu[i];
    }
}
```

观察线性筛的过程，当 $i\bmod \operatorname{pri}_j=0$ 时，$\operatorname{pri}_j\mid i \Rightarrow (\operatorname{pri}_j)^2\mid i\cdot \operatorname{pri_j}$，这种情况下 $\mu(i\cdot \operatorname{pri_j})=0$.

否则，$\operatorname{pri_j}$ 是第一次对 $i\cdot \operatorname{pri_j}$ 产生贡献，$\mu(i\cdot \operatorname{pri_j})=-\mu(i)$.

# 欧拉函数与欧拉定理

## 定义

给定 $n\in \Bbb{N}^{*}$，欧拉函数是小于等于 $n$ 的所有数中与 $n$ 互质的数的个数，记为 $\varphi(n)$.

形式化地，有：
$$
\varphi(n)=\sum_{i=1}^{n} [\gcd(i,n)=1]
$$

## 积性函数

**积性函数**：满足 $f(m\cdot n)=f(m)\cdot f(n)$，$\gcd(m,n)=1$.

**完全积性函数**：满足 $f(m\cdot n)=f(m)\cdot f(n)$，不要求 $m,n$ 互质。

可以证明，**欧拉函数是积性函数**。

## 求单个数的欧拉函数

有结论：
$$
\varphi(x)=x(1-\frac{1}{p_1})(1-\frac{1}{p_2})\cdots(1-\frac{1}{p_n})
$$
其中 $p_1,p_2,\cdots,p_n$ 是 $x$ 的所有质因数，例如：$\varphi(12)=12\times(1-\frac{1}{2})\times(1-\frac{1}{3})=4$.

推导过程如下。

首先，显然 $\varphi(1)=1$，且若 $p$ 是质数，有 $\varphi(p)=p-1$.

给定质数 $p$ 和正整数 $k$，考虑计算 $\varphi(p^k)$。

根据质数的性质，只有 $p$ 的倍数与 $p^k$ 不互质，因此一种计算方法是：用总数 $p^k$ 减去 $p$ 的倍数个数，即：
$$
\varphi(p^k)=p^k-p^{k-1}=p^k\times (1-\frac{1}{p})
$$
根据积性函数性质可得：
$$
\begin{aligned}
\varphi(x)&=\varphi(p_1^{k_1})\times\varphi(p_2^{k_2})\times\cdots\times\varphi(p_n^{k_n})\\
&=p_1^{k_1}(1-\frac{1}{p_1})p_2^{k_2}(1-\frac{1}{p_2})\cdots p_n^{k_n}(1-\frac{1}{p_n})\\
&=x(1-\frac{1}{p_1})(1-\frac{1}{p_2})\cdots(1-\frac{1}{p_n})
\end{aligned}
$$

```cpp
inline int phi(int n) {
	int res = n;
	for(int i = 2; i * i <= n; i++) { // 分解质因数
		if(n % i == 0) { // i 是 n 的质因子
			res = res / i * (i - 1);
			while(n % i == 0) n /= i;
		}
	}
	if (n > 1) //最后可能还剩一个最大的质因子
		res = res / n * (n - 1);
	return res;
}
```

## 欧拉定理

## 内容

任意选取 $a,n\in \Bbb{N}^{*}$ 满足 $\gcd(a, n)=1$，则有：
$$
a^{\varphi(n)}\equiv1\pmod n
$$

## 证明



## 费马小定理

对任意给定的正整数 $a$ 和质数 $p$，都有：
$$
a^{p-1}\equiv 1\pmod p
$$
容易发现，这是欧拉定理的特殊情况，证明不再赘述。

# 扩展欧几里得算法

## 概述

扩展欧几里得算法，用于求解**一元一次不定方程** $ax+by=\gcd(a,\,b)$，其中 $a,b$ 为已知系数，$x,y$ 为未知数。

## 裴蜀定理

**设 $a,b$ 是不全为零的整数，则存在整数 $x,y$ 使得 $ax+by=\gcd(a, b)$**.

因此，扩展欧几里得算法所求解的不定方程一定有整数解。

## 特解

要求出一组满足方程条件的整数解 $(x,y)$.

考虑欧几里得算法的过程，$\gcd(a, b)=\gcd(b, a\bmod b)$.

记 $d=\gcd(a, b)$，则要求解的方程为 $ax+by=d$.

**关注欧几里得算法的递归过程**，$(a,b)$ 递归至 $(b, a\bmod b)$ 后，有方程 $b\cdot x_1+(a\bmod b)\cdot y_1=d$.

现在要找到 $(x,y)$ 与 $(x_1,y_1)$ 之间的等量关系，即：**从递归的深层推到浅层**。

根据 $\bmod$ 运算的定义：
$$
b\cdot x_1+(a\bmod b)\cdot y_1=d
\Rightarrow  b\cdot x_1+(a-\left\lfloor\frac{a}{b}\right\rfloor\times b)\cdot y_1=d
$$
整理得到：
$$
\begin{aligned}
d&=b\cdot x_1+(a-\left\lfloor\frac{a}{b}\right\rfloor\times b)\cdot y_1\\
&=b\cdot x_1+a\cdot y_1-(\left\lfloor\frac{a}{b}\right\rfloor\times b)\cdot y_1\\
&=a\cdot y_1+b\cdot(x_1-\left\lfloor\frac{a}{b}\right\rfloor\times y_1)
\end{aligned}\\
$$
与方程 $ax+by=d$ 对应，可知一种可行的**对应关系**为：
$$
\left\{
\begin{array}{l}
x=y_1\\
y=x_1-\left\lfloor\frac{a}{b}\right\rfloor\times y_1
\end{array}
\right.
$$
按照这样的等量关系，在递归过程中计算即可。

欧几里得算法的递归基为 $b=0$，此时 $\gcd(a,b)=a$，方程 $ax+by=d$ 的解为 $x=0,y=1$.

~~~cpp
int exgcd(int a, int b, int &x, int &y) { // 函数返回值为gcd(a,b)
	if(b == 0) {
		x = 1, y = 0;
		return a;
	}
	int r = exgcd(b, a % b, x, y); //先递归
	int t = x; //再计算x,y
	x = y;
	y = t - a / b * y;
	return r;
}
~~~

**可以证明，这种方法求出 $ax+by=d$ 的特解 $(x,y)$ 必满足 $|x|\leq b$，$|y|\leq a$**.

## 通解

假设已求出特解为 $(x_0,y_0)$，满足 $ax_0+by_0=\gcd(a,\,b)$.

仍然记 $d=\gcd(a, b)$，则有
$$
a(x_0+k\frac{b}{d})+b(y_0-k\frac{a}{d})=d,k\in \Bbb{Z}
$$
显然，$k\dfrac{b}{d}$ 和 $k\dfrac{a}{d}$ 是在保证解仍为整数的前提下，可取到的**最小整系数**。

所以方程的通解为：
$$
\left\{
\begin{array}{l}
x=x_0+k\frac{b}{d}\\y=y_0-k\frac{a}{d}
\end{array}
\right.
$$
对于任意二元一次不定方程 $ax+by=c,(a,b,c\in \Bbb{Z})$，当且仅当 $d\mid c$ 时才有整数解。

在求出 $ax+by=\gcd(a,b)$ 后，可以得到方程特解为 $(\frac{c}{d}x_0, \frac{c}{d}y_0)$，且有通解为：
$$
\left\{
\begin{array}{l}
x=\frac{c}{d}x_0+k\frac{b}{d}\\y=\frac{c}{d}y_0-k\frac{a}{d}\end{array}
\right.
$$

## 最小非负整数解

由通解形式，可用 `(x % (b/d) + (b/d)) % (b/d)` 求得最小非负整数解。

这是因为在 C++ 语言中，负整数 $x$ 进行运算 `x % p` 的结果满足 $-p<s\leq 0$.

## 同余方程

同余方程，指形如 $ax\equiv b\pmod c$ 的方程。其中 $a,b,c$ 是已知量，$x$ 是未知量。

注意到：
$$
ax\equiv b\pmod c\Leftrightarrow xa+kc=b
$$
于是转化为一般的**一元一次不定方程**，可用**扩展欧几里得算法**求解。

# 乘法逆元

## 定义

给定两个整数 $a$ 和 $p$，假设存在 $x$ 使得 $ax\equiv 1\pmod p$，则称 $x$ 为 $a$ 关于 $p$ 的**乘法逆元**。

乘法逆元又称数论倒数，是模意义下的除法，可记为 $x\equiv a^{-1}\pmod p$.

可以证明，$a$ 关于 $p$ 的逆元存在的充要条件是 $\gcd(a,p)=1$.

## 求一个数的逆元

将上面式子变形，变成$ax+kp=1$，用扩展欧几里得能够求出一个$x$.

若 $p$ 是质数，可利用**费马小定理**求逆元。
$$
a^{p-1}\bmod p=(a^{p-2}\times a)\bmod p=(x\times a)\bmod p=1
$$
所以 $a$ 关于 $p$ 的逆元是 $a^{p-2}\bmod p$ 的值，用快速幂优化，时间复杂度为 $\mathcal{O}(\log n)$.

代码实现不难。

## 求一组数的逆元

给定 $a_1, a_2, \cdots, a_n$ 和模数 $p$，若他们都与 $p$ 互质，则可以在 $\mathcal{O}(n+\log a_i)$ 的时间复杂度内计算出每个数的逆元。

预处理前缀积 $\operatorname{s}_i=(\prod_{i=1}^i a_i) \bmod p$，先计算出前缀积数组的逆元 $t_i$.

$t_n$ 可通过扩展欧几里得算法或快速幂计算得到，对于 $t_{1\sim n-1}$，可以递推计算：

在模意义下，有：
$$
t_i\equiv \frac{1}{a_1a_2\cdots a_i}\pmod p\\
t_{i+1}\equiv\frac{1}{a_1a_2\cdots a_i a_{i+1}}\pmod p
$$
所以：
$$
t_i\equiv \frac{1}{a_1a_2\cdots a_i}\equiv \frac{a_{i+1}}{a_1a_2\cdots a_i a_{i+1}}\equiv t_{i+1}\cdot a_{i+1}\pmod p\\
$$
求出 $t_i$ 后，设 $a_i$ 的逆元为 $\operatorname{inv_i}$，则 $\operatorname{inv}_1=t_1$，其余项可以直接计算：
$$
\operatorname{inv}_i\equiv \frac{a_1a_2\cdots a_{i-1}}{a_1a_2\cdots a_{i-1}a_i}\equiv t_i\cdot s_{i-1}\pmod p
$$

## 组合数取模

按照上面的方法，代入 $a_i=i$，求出 $s_i=i!$，$t_i={s_i}^{-1}=\dfrac{1}{i!}$.

根据组合数的定义：
$$
C_n^m\equiv \binom{n}{m}\equiv \frac{n!}{m!(n-m)!}\equiv s_n\cdot t_m\cdot t_{n-m}\pmod p
$$

~~~cpp
const int N = 1e6, mod = 1e9 + 7;
inline int power(int x, int k) {
	int ret = 1;
	while(k) {
		if(k & 1) ret = ret * x % mod;
		x = x * x % mod, k >>= 1;
	}
	return ret;
}
int fac[N + 10], inv[N + 10];
inline void init() {
	fac[0] = 1;
	for(int i = 1; i <= N; i++) fac[i] = fac[i - 1] * i % mod;
	inv[N] = power(fac[N], mod - 2);
	for(int i = N - 1; i >= 0; i--) inv[i] = inv[i + 1] * (i + 1) % mod;
}
~~~

# 中国剩余定理

> 有物不知其数，三三数之剩二，五五数之剩三，七七数之剩二。问物几何? 

这个问题就可以转化为一个同余方程组。设 $x$ 为物品数量，则有：
$$
\left\{
\begin{align}{}
x\equiv 2\pmod 3\\
x\equiv 3\pmod 5\\
x\equiv 2\pmod 7
\end{align}{}
\right.
$$


## 内容

中国剩余定理一元同余线性方程组：
$$
\left\{
\begin{aligned}&x\equiv a_1\pmod {m_1}\\&x\equiv a_2\pmod {m_2}\\&\cdots\\&x\equiv a_n\pmod {m_n}\end{aligned}
\right.
$$
假设整数 $m_1,m_2,\cdots,m_n$ **两两互质**，则方程组必有解，通解可以构造如下：

设 $M=\prod m_i$，并设 $M_i=\dfrac{M}{m_i}$， $t_i\equiv M_i^{-1}\pmod {m_i}$，有通解为：

$$
x=a_1t_1M_1+a_2t_2M_2+\cdots+a_nt_nM_n+kM=\sum_{i-1}^{n}a_it_iM_i+kM
$$

这不难理解。在 $\bmod m_i$ 意义下，整数 $j$ 满足：
$$
a_jt_jM_j\equiv \left\{
\begin{array}{}
0, &j\neq i\\
a_j, &j=i
\end{array}
\right. \pmod {m_i}
$$
因为 $j\neq i$ 时，$M_j\equiv 0\pmod {m_i}$，且有 $t_iM_i\equiv 1\pmod {m_i}$.

## 代码实现

程序的返回值为最小整数解。

~~~cpp
int exgcd(int a, int b, int &x, int &y) {
	if (b == 0) {
		x = 1;
		y = 0;
		return a;
	}
	int r = exgcd(b, a % b, x, y);
	int t = x;
	x = y;
	y = t - a / b * y;
	return r;
}
int a[101], m[101];
inline int CRT(int n) {
	int M = 1;
	int ans=0;
	for(int i = 1; i <= n; i++) {
		M *=m [i];
	}
	for(int i = 1; i <= n; i++) {
		int x, y;
		int Mi = M / m[i];
		exgcd(Mi, m[i], x, y);
		ans = (ans + Mi * x * a[i]) % M;
	}
	if(ans < 0) ans += M;
	return ans;
}
~~~

# 扩展中国剩余定理

## 概述

同样用于求解同余方程组，与中国剩余定理的区别是**模数不一定互质**。

## 内容

考虑两个同余方程：
$$
\left\{
\begin{aligned}x\equiv a_1 \pmod{m_1}\\
x\equiv a_2\pmod{m_2}
\end{aligned}
\right.
$$
可以写为：
$$
\left\{
\begin{array}{}
x+k_1m_1=a_1&&(1)\\
x+k_2m_2=a_2&&(2)
\end{array}
\right.
$$
$(1)-(2)$ 得到：
$$
m_1k_1-m_2k_2=a_1-a_2
$$
可见，方程形如 $ax+by=c$，所以有解条件是 $\gcd(m1,m2)\mid (a_1-a_2)$.

用扩展欧几里得算法求得可行解 $(K_1, K_2)$，因此 $K_1$ 的通解为：
$$
K_1+\frac{m_2}{d}\cdot C
$$
代入 $(1)$，得到：
$$
x+K_1m_1+\frac{m_2m_1}{d}\cdot C=a_1
$$
写成同余方程的形式：
$$
x\equiv a_1-K_1m_1 \pmod{\operatorname{lcm}(m_1, m_2)}
$$
这样就把两个同余方程合并成了一个。

同理，可以将 $n$ 个方程组成的同余方程组化为一个同余方程，最后用扩展欧几里得算法求解 $x$.

# 卢卡斯定理

对于**质数** $p$，有：
$$
\binom{n}{m}\bmod p = \binom{\left\lfloor\frac{n}{p}\right\rfloor}{\left\lfloor\frac{m}{p}\right\rfloor} \cdot \binom{n\bmod p}{m\bmod p}\bmod p
$$
当 $p\leq 10^6$ 且 $n,m$ 较大时实用。

# 扩展卢卡斯定理

123