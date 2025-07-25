---
title: 本质不同子序列的计数问题
publishDate: 2022-04-04
description: '本质不同子序列的计数问题。包含多种 DP 方法和区间本质不同子序列计数方法。
'
tags:
  - oi_note

language: '中文'
---

# 〇

**子序列**：对于一个给定序列，按原本顺序抽取若干元素，构成的序列即为序列的子序列。

**本质不同**：对于两个子序列 $\{b_n\}$ 和 $\{c_n\}$，若其元素个数不同或存在 $b_i\neq c_i$，则称它们是本质不同的。

**本质不同子序列**：要求子序列必须是**非空**的。

对于两个子序列 $a,b$，**$a+b$ 表示将 $a$ 和 $b$ 首尾相连**，构成一个新的子序列。

# 一

给定长度为 $n$ 的序列 $\{a_n\}$，求在 $\{a_n\}$ 中有多少个本质不同的子序列。

考虑设 $f_i$ 表示截至第 $i$ 个数， 有多少个本质不同子序列；设 $\operatorname{last}_{i}$ 表示数值 $i$ 的上一次出现位置，初始值为 $0$，在遍历更新 $f$ 时随时更新。

则有转移方程：

$$
f_i=\left\{
\begin{array}{lr}
2\times f_{i-1}+1, &\operatorname{last}_{a_i}=0\\
2\times f_{i-1} - f_{\operatorname{last}_{a_i} - 1}, &\operatorname{last}_{a_i}\neq0\\
\end{array}
\right.
$$

理解：

1. 若之前从未出现过 $a_i$，对任意之前出现过的子序列 $s$，$s+a_i$ 都是新的子序列；另外，$a_i$ 本身也可以作为一个新的子序列。
2. 若之前出现过 $a_i$，对于一个仅包含 $\operatorname{last}_i$ 之前的元素的子序列 $s$，$s+i$ 已经被计算过了，容斥减去。


# 二

给定长度为 $n$ 的 **01 序列** $\{a_n\}$，求在 $\{a_n\}$ 中有多少个本质不同的子序列。

时间复杂度 $\mathcal{O}(n)$.

考虑设 $f_{i,0/1}$ 表示截至第 $i$ 个数，有多少以 $0/1$ 结尾的本质不同子序列。

则有转移方程：

$$
f_{i,0}=\left\{
\begin{array}{lr}
f_{i-1,0}+f_{i-1,1}+1, &a_i=0\\
f_{i-1,0}, & a_i=1\\
\end{array}
\right.
$$

$$
f_{i,1}=\left\{
\begin{array}{lr}
f_{i-1,1}, &a_i=0\\
f_{i-1,0}+f_{i-1,1}+1, & a_i=1\\
\end{array}
\right.
$$

理解：

若当前位置 $a_i=1$，考虑以 $1$ 结尾的子序列。对于 $f_{i-1}$ 包含的所有本质不同子序列，都直接在后面添上一个 $1$，这些子序列依然不重不漏，除此之外，以 $1$ 结尾的子串还可以为单独的一个 $1$。

对于由 $0$ 结尾的子序列，$a_i$ 不能造成任何贡献。

由此，此时 $f_{i,1}=f_{i-1,0}+f_{i-1,1}+1$，$f_{i,0}=f_{i-1,0}$.

# 三

给定长度为 $n$ 的 **01 序列** $\{a_n\}$，$m$ 次询问子段 $a_l\sim a_r$ 中有多少个本质不同的子序列。

时间复杂度 $\mathcal{O}(m)\sim \mathcal{O}(m\log n)$.

在**二**的基础上，注意到转移只与 $f_{i-1}$ 有关，因此可以构造 $3\times 3$ 的矩阵，用**矩阵快速幂**优化。

若 $a_i=0$，

$$
\begin{bmatrix} f_{i-1,1}& f_{i-1,0} &1\end{bmatrix}\times
\begin{bmatrix}
1 &1 &0\\
0 &1 &0\\
0 &1 &1\\
\end{bmatrix}=
\begin{bmatrix} f_{i,1}& f_{i,0} &1\end{bmatrix}
$$

若 $a_i=1$，

$$
\begin{bmatrix} f_{i-1,1}& f_{i-1,0} &1\end{bmatrix}\times
\begin{bmatrix}
1 &0 &0\\
1 &1 &0\\
1 &0 &1\\
\end{bmatrix}=
\begin{bmatrix} f_{i,1}& f_{i,0} &1\end{bmatrix}
$$

形式化地，设 $M_0=\begin{bmatrix}1 &1 &0\\0 &1 &0\\0 &1 &1\\\end{bmatrix}$，$M_1=\begin{bmatrix}1 &0 &0\\1 &1 &0\\1 &0 &1\\\end{bmatrix}$，则答案矩阵为 $\prod\limits_{i=l}^rM_{a_i}$.

可以预处理逆矩阵，用前缀积进一步优化。

# 四

给定长度为 $n$ 的 **01 序列** $\{a_n\}$，$m$ 次询问操作：

1. 把数列子段 $a_l\sim a_r$ 中的所有数取反。
2. 求数列子段 $a_l\sim a_r$ 中有多少个本质不同的子序列。

时间复杂度 $\mathcal{O}(m\log n)$.



观察可知，区间翻转等价于交换 $3\times 3$ 矩阵的前两列。

用线段树维护区间矩阵乘积，懒标记维护区间翻转操作。

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;
inline int read() {
	int ret = 0, ch = getchar();
	while(ch < '0' || ch > '9') ch = getchar();
	while(ch <= '9' && ch >= '0') {
		ret = ret * 10 + ch - '0';
		ch = getchar();
	}
	return ret;
}
const int MAXN = 1e5 + 10, mod = 1e9 + 7;
int n, m, a[MAXN], f[MAXN];
//dp[i][1] = dp[i-1][1] + dp[i-1][0] + 1, dp[i][0] = dp[i-1][0], a[i]=1
//dp[i][1] = dp[i-1][1], dp[i][0] = dp[i-1][0] + dp[i-1][1] + 1, a[i]=0
struct Matrix{
	int n, m, a[4][4];
	Matrix() { memset(a, 0, sizeof(a)); }
	friend Matrix operator * (const Matrix &A, const Matrix &B) {
		Matrix C;
		C.n = A.n, C.m = B.m;
		for(int i = 1; i <= C.n; i++) {
			for(int j = 1; j <= C.m; j++) {
				for(int k = 1; k <= A.m; k++) {
					C.a[i][j] = (C.a[i][j] + A.a[i][k] * B.a[k][j] % mod) % mod;
				}
			}
		}
		return C;
	}
	inline void rev() {
		for(int i = 1; i <= 3; i++) swap(a[i][1], a[i][2]);
		for(int i = 1; i <= 3; i++) swap(a[1][i], a[2][i]);
	}
} M[2];
inline Matrix unit(int n) {
	Matrix R; R.n = R.m = n;
	for(int i = 1; i <= n; i++) R.a[i][i] = 1;
	return R;
}
inline void Init_M() {
	M[0].n = M[0].m = M[1].n = M[1].m = 3;
	M[0].a[1][1] = 1, M[0].a[1][2] = 1, M[0].a[1][3] = 0;
	M[0].a[2][1] = 0, M[0].a[2][2] = 1, M[0].a[2][3] = 0;
	M[0].a[3][1] = 0, M[0].a[3][2] = 1, M[0].a[3][3] = 1;
	M[1].a[1][1] = 1, M[1].a[1][2] = 0, M[1].a[1][3] = 0;
	M[1].a[2][1] = 1, M[1].a[2][2] = 1, M[1].a[2][3] = 0;
	M[1].a[3][1] = 1, M[1].a[3][2] = 0, M[1].a[3][3] = 1;
}
Matrix t[MAXN << 2]; int tag[MAXN << 2], lc[MAXN << 2], rc[MAXN << 2];
#define ls(x) ((x) << 1)
#define rs(x) ((x) << 1 | 1)
inline void push_up(int id) { t[id] = t[ls(id)] * t[rs(id)]; }
inline void push_down(int id) {
	if(!tag[id]) return ;
	t[ls(id)].rev(), t[rs(id)].rev();
	tag[ls(id)] ^= 1, tag[rs(id)] ^= 1;
	tag[id] = 0;
}
void build(int id, int l, int r) {
	tag[id] = 0;
	lc[id] = l; rc[id] = r;
	if(l == r) {
		t[id] = M[a[l] == 1];
		return ;
	}
	int mid = (l + r) >> 1;
	build(ls(id), l, mid);
	build(rs(id), mid + 1, r);
	push_up(id);
}
void update(int id, int l, int r, int X, int Y) {
	if(X <= l && r <= Y) {
		tag[id] ^= 1;
		t[id].rev();
		return;
	}
	push_down(id);
	int mid = (l + r) >> 1;
	if(X <= mid) update(ls(id), l, mid, X, Y);
	if(Y > mid) update(rs(id), mid + 1, r, X, Y);
	push_up(id);
}
Matrix query(int id, int l, int r, int X, int Y) {
	if(X <= l && r <= Y) return t[id];
	push_down(id);
	int mid = (l + r) >> 1; Matrix ret = unit(3);
	if(X <= mid) ret = ret * query(ls(id), l, mid, X, Y);
	if(Y > mid) ret = ret * query(rs(id), mid + 1, r, X, Y);
	return ret;
}
void sol() {
	Init_M();
	build(1, 1, n);
	while(m--) {
		int opt = read(), l = read(), r = read(), ans;
		if(opt == 1) {
			update(1, 1, n, l, r);
		} else {
			Matrix R = query(1, 1, n, l, r);
			ans = (R.a[3][1] + R.a[3][2]) % mod;
			printf("%lld\n", ans);
		}
	}
}
signed main() {
	n = read(), m = read();
	for(int i = 1; i <= n; i++) a[i] = read();
	sol();
	return 0;
}
```

