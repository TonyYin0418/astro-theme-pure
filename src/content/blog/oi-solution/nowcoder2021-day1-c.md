---
title: 【2021牛客OI赛前集训营】与巨
publishDate: 2021-10-05
description: '算法题解'
tags:
  - oi_solution

language: '中文'
---

题目来源：[2021牛客OI赛前集训营-提高组（第一场）](https://ac.nowcoder.com/acm/contest/20106)

## 题意

定义序列 $f$：
$$
f_i=
\left\{
\begin{array}{lr}
1,&i=1\\
f_{i-1}\times 2+1, &i>1
\end{array}
\right.
$$
定义函数 $G(x)=\min\limits_{f_i\geq x}(f_i)$.

定义序列 $\operatorname{dp}$：
$$
\operatorname{dp}_i=
\left\{
\begin{array}{lr}
0,&i=0\\
\max\left\{
\begin{array}{lr}
\operatorname{dp}_{i-1}\\
[(i\times c)\&G(i)==i]\times i
\end{array}
\right\}, &i>0
\end{array}
\right.
$$
求 $\sum_{i=0}^n{\operatorname{dp}_i}\bmod 998244353$.



## 分析

直接暴力有 $\rm{20pts}$.

观察 $f_i$ 的形式，$G(i)$ 一定可以表示为 $2^{t+1}-1,t\in \Bbb{N^*}$.

把二进制形式写出来，$x\&G(i)$ 等价于 $x\bmod 2^{t+1}$.

因此，**当 $i\in[2^t, 2^{t+1}-1]$ 时**：
$$
\begin{array}{ll}
& (i\times c) \& G(i)=i\\
\Rightarrow & (i\times c)\bmod 2^{t+1}=i\\
\Rightarrow & i\times (c-1)\equiv0\pmod{2^{t+1}}
\end{array}
$$
把 $(c-1)$ 中二的幂次提出来，设 $c-1=2^p\times x$，则 $i$ 需要含有因子 $2^{t+1-p}$，即 $i\equiv 0\pmod{2^{t+1-p}}$。

**注意：** 有特殊情况，若 $c-1=0$，条件恒成立，答案直接就是 $\sum_{i=0}^n{i}$，需要单独判断。

设 $m$ 为二进制下 $n$ 的长度 $|n|$.

考虑枚举上面的 $t$，由于 $m$ 位二进制数的取值范围是 $[0, 2^m-1]$，而 $G(i)=2^{t+1}-1$，因此 $t\in[0, m-1]$。

分两种情况讨论。

#### 当 $t\neq m-1$ 时

$[2^t, 2^{t+1}-1]$ 内的数，最高位都为 $t$.

$i$ 需要含有因子 $2^{t+1-p}$，为了方便表示，设 $g=t+1-p$.

$i$ 含有 $2^g$，这样的数为：
$$
2^t,\quad 2^t+2^g,\quad  2^t+2\cdot 2^g,\quad  2^t+3\cdot 2^g,\cdots,\quad 2^t+x\cdot 2^g
$$
$x$ 就是最后一项的系数，满足 $2^t+(x+1)\cdot2^g=2^{t+1}$.

上面共有 $x+1$ 项，是等差数列，可以直接求和。

依题意，每个数对答案有 $2^g$ 次贡献，还要对 $\operatorname{sum}$ 乘上 $2^g$。

**注意：** 当 $g=t+1$，即 $c\bmod 2=0$ 时，上面的等差数列不存在，答案直接为 $0$。为了方便实现，可以在代码的最前面特判这种情况。

#### 当 $t=m-1$ 时

和上面类似。不过由于 $2^{t+1}>n$，在 $[2^t, 2^{t+1}-1]$ 当中有一部分数比 $n$ 大，因此 $x$ 的计算方法需要改变。

具体地，我们需要满足 $2^t+x\times 2^g\leq n$，因此，$x=\lfloor\frac{n}{2^g}\rfloor-2^{t-g}$.

同样地，对这个 $x+1$ 项的等差数列求和，乘上 $2^g$ 加入贡献。

在这个计算方法中，等差数列的最后一项也被计数了 $2^g$ 次，认为其对 $[2^t+x\cdot 2^g, 2^t+(x+1)\cdot 2^g-1]$ 这个范围内都有贡献。

然而区间右端点可能 $>n$，所以要减去 $2^t+(x+1)\cdot 2^g-1-n$ 次贡献，每次贡献的值是 $2^t+x\times 2^g$。

## 代码

注意等差数列求和的时候，要用 $2$ 的逆元代替除法.

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int MAXLEN = 1e7 + 10, mod = 998244353, inv2 = 499122177;
int T, m, c;
char s[MAXLEN];
int pow_2[MAXLEN];
inline int calc(int st, int ed, int x) {//首项、末项、项数
	if(x <= 0) return 0;
	return (st + ed) * inv2 % mod * x % mod;
}
void solve_0() {
	int n = 0;
	for(int i = 1; i <= m; i++) n = ((n << 1) + s[i] - '0') % mod;
	printf("%lld\n", n * (n + 1) % mod * inv2 % mod);
}
signed main() {
	pow_2[0] = 1;
	for(int i = 1; i <= 1e7; i++)
		pow_2[i] = (pow_2[i - 1] << 1) % mod;
	scanf("%lld", &T);
	while(T--) {
		scanf("%s%lld", (s+1), &c);
		if(c % 2 == 0) {
			printf("0\n");
			continue;
		}
		if(c - 1 == 0) {
			solve_0();
			continue;
		}
		m = strlen(s + 1); c = c - 1;
		int p = 0, ans = 0;
		while(c != 0 && c % 2 == 0) p++, c /= 2;
		for(int t = 0; t <= m - 1; t++) {
			int g = max(0ll, t + 1 - p);
			//c%2==0的情况在上面处理过了，所以可以像下面这样对等差数列求和
			if(t < m - 1) { //t < m - 1时
				ans += pow_2[g] * calc(pow_2[t], pow_2[t+1]-pow_2[g], pow_2[t+1-g]-pow_2[t-g]) % mod;
				ans %= mod;
			} else { //t = m - 1时
				//n/(2^g)下取整，也就是二进制下左边的前n-g位的值
				int x = 0;
				for(int i = 1; i <= m - g; i++)
					x = ((x << 1) + s[i] - '0') % mod;
				x = (x + mod - pow_2[t - g]);
				ans += pow_2[g] * calc(pow_2[t], (pow_2[t] + x * pow_2[g]) % mod, x + 1);
				ans %= mod;
				int n = 0, r = (pow_2[t] + (x + 1) * pow_2[g] % mod - 1) % mod;
				for(int i = 1; i <= m; i++)
					n = ((n << 1) + s[i] - '0') % mod;
				ans -= (r - n) * (pow_2[t] + x * pow_2[g] % mod) % mod;
				ans %= mod;
			}
		}
		ans = (ans + mod) % mod;
		printf("%lld\n", ans);
	}
	return 0;
}
```

