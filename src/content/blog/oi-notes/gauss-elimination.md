---
title: 高斯消元
publishDate: 2021-10-16
description: '2021.10.16，CSP 赛前专题分享，文字版讲稿'
tags:
  - oi_note

language: '中文'
---

# 高斯消元法

> 高斯消元法（Gauss-Jordan elimination），可以用于解决：线性方程组求解、行列式求值、矩阵求逆等问题。

给定一个线性方程组，要求解。

## 一些定义

变量 $=$ 元 $=$ 变元 $=$ 未知数。

对于一个 $m$ 元线性方程组，已知系数 $a_{ij}$​，未知量 $x_i$​，即：
$$
\left\{
\begin{array}{}
a_{1,1}x_1+a_{1,2}x_2+\cdots+a_{1,m}x_m=c_1 &(1)\\
a_{2,1}x_1+a_{2,2}x_2+\cdots+a_{2,m}x_m=c_2 &(2)\\
\cdots\\
a_{n,1}x_1+a_{n,2}x_2+\cdots+a_{n,m}x_m=c_n &(n)
\end{array}
\right.
$$
将简记为一个 $n$ 行 $m$ 列的矩阵：
$$
\left[\begin{matrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,m} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,m} \\
\cdots & \cdots & \cdots & \cdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,m}
\end{matrix} \right]
$$
我们称这个矩阵为 **系数矩阵**。

在系数矩阵右侧，**加一列常数项**，称其为 **增广矩阵**：
$$
\left[\begin{matrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,m} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,m} \\
\cdots & \cdots & \cdots & \cdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,m}
\end{matrix} \middle|
\begin{matrix}
c_1 \\
c_2 \\
\cdots\\
c_n
\end{matrix} \right]
$$
竖线 $\mid$ 没有其他含义，只是为了区分等式左右两侧。两侧同属一个矩阵。

为了方便表示，**后面出现的 $c_i$ 可能会用 $a_{i, m+1}$ 代替**。

## 简单情况

从**简单情况**开始考虑，假设恰好 $n=m$，且每个方程都有意义，方程组形如：
$$
\left\{
\begin{array}{r}
&a_{1,1}x_1&+ &a_{1,2}x_2&+\cdots+ &a_{1, n-1}x_{n-1} &+ &a_{1,n}x_n=c_1 &(1)\\
&0\cdot x_1&+ &a_{2,2}x_2&+\cdots+ &a_{2, n-1}x_{n-1} &+ &a_{2,n}x_n=c_2 &(2)\\
&0\cdot x_1&+ &0\cdot x_2&+\cdots+ &a_{3, n-1}x_{n-1} &+ &a_{3,n}x_n=c_2 &(3)\\
&&&&+\cdots+\\
&0\cdot x_1&+ &0\cdot x_2&+\cdots+ &0\cdot x_{n-1} &+ &a_{n,n}x_n=c_n &(n)
\end{array}
\right.
$$
矩阵为：
$$
\left[\begin{matrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n-1} & a_{1,n} \\
0 & a_{2,2} & \cdots & a_{2,n-1} & a_{2,n} \\
0 & 0 & \cdots & a_{3,n-1} & a_{3,n} \\
\cdots & \cdots & \cdots & \cdots & \cdots \\
0 & 0 & \cdots & 0 & a_{n,n}
\end{matrix} \middle|
\begin{matrix}
a_{1,n+1} \\
a_{2,n+1} \\
a_{3,n+1} \\
\cdots\\
a_{n,n+1}
\end{matrix} \right]
$$
矩阵的左下角都是 $0$.

对于这种倒三角矩阵，最后一行代表的方程为：$a_{nm}\cdot x_m=a_{n,m+1}$，可以直接求出 $x_m=\frac{a_{n, m+1}}{a_{nm}}$.

之后，我们再将 $x_m$ 代入倒数第二行的方程：$a_{n-1,m-1}\cdot x_{m-1}+a_{n-1,m}\cdot x_m=a_{n-1, m+1}$，求出 $x_{m-1}$.

以此类推，就可以求出所有的未知量了。

考虑如何**将原矩阵变换为倒三角矩阵**。



这就和一般的消元法解方程一样，要让第 $x_i$ 个未知数只存在于前 $i$ 个方程（其它方程中 $x_i$ 系数均为 $0$）.

枚举列，也就是枚举未知数。当我们枚举到第 $c$ 列的时候，我们要让 $\sum_{i=c+1}^{n}[a_{i, c}\neq0]=0$.

所以对于第 $i\in [c+1, n]$ 行，整行一起变换，使 $a_{i,c}=0$：
$$
\forall j\in [1,m+1],\quad a_{i, j}\leftarrow a_{i, j}-a_{c, j}\cdot\frac{a_{i, c}}{a_{c, c}}
$$
由于 $\forall j\in[1, c-1]，a_{c, j}=0$，所以上式与 $j\in[c, m+1]$ 是等价的。

之后进行回带，把所有无关项移到等式右边。

```cpp
double A[MAXN][MAXN], ans[MAXN];
void Gauss() {
	for(int c = 1; c <= n; c++) {
		for(int i = c + 1; i <= n; i++) {
			double t = A[i][c] / A[c][c];
			for(int j = c + 1; j <= n + 1; j++) {
				A[i][j] -= A[c][j] * t;
			}
		}
	}
    for(int i = n; i >= 1; i--) {
        for(int j = i + 1; j <= n; j++) a[i][n + 1] -= a[i][j] * ans[j];
        ans[i] = a[i][n + 1] / a[i][i];
    }
}
```

## 提高精度

在消元过程中，对于第 $i$ 个主元，我们默认选择第 $i$ 行的 $a_{i, i}$ 作为保留的元素。

为了减小误差，我们在第 $i$ 列的 $a_{i, j}(j\geq i)$ 中选一个最大的数作为保留的元素，之后交换第 $i$ 行和第 $j$ 行。

显然，这样有助于精度的控制。

## 扩大适用范围

前面的方法基于 $n=m$，且方程组存在唯一解。然而，方程个数与未知数不一定相等，解的个数也不一定唯一，下面给出**更普适的方法**。

#### 判断解的个数

容易发现，由 $n$ 个方程构成的 $m$ 元线性方程组，解的个数只可能为：$0, 1, \infty$.

为了方便判断，我们引入**自由元**的定义：取值不确定的变元。

当我们枚举到第 $i$ 个变量，且前 $i$ 个变量都不是自由元时，如果第 $i\sim n$ 行中**这个变量的系数均为 $0$**，那么第 $i$ 个变量是自由元。
$$
\left[\begin{matrix}
a_{1,1} & a_{1,2} & a_{1, 3}      & a_{1,4} & a_{1,5} \\
0       & a_{2,2} & a_{2, 3}      & a_{2,4} & a_{2,5} \\
0       & 0       & \color{red}{0}& a_{3,4} & a_{3,5} \\
0       & 0       & 0             & a_{4,4} & a_{4,5} \\
0       & 0       & 0             & 0       & a_{5,5}
\end{matrix} \middle |
\begin{matrix}
c_{1} \\
c_{2} \\
c_{3} \\
c_{4} \\
c_{5}
\end{matrix} \right]
$$
 上面的例子中，$x_3$ 即为自由元。可以发现，当 $x_4, x_5$ 被确定之后，第三行的方程可以记为：
$$
0\cdot x_3=c_3-x_4\cdot a_{3, 4}-x_5\cdot a_{3, 5}=C
$$
若 $C=0$，则 $x_3$ 可以在 $\Bbb{R}$ 中任取。

若 $C\neq 0$，则 $x_3$ 一定无解，方程组也一定无解。

**另外**，存在一个自由元，其值可以任取，不代表方程组有无穷组解。因为若有其它变量是无解的，则方程组无解。

> 出现自由元这种情况，说明有些方程之间是线性相关的，所以加减消元之后就消没了。

#### 解决 $n\neq m$ 的情况

在枚举变量的过程当中，会发现有些变量是自由元。这时我们**应当跳过这一列，并且保持行不变**，处理下一个变元。

因此我们用指针 $\operatorname{row}$ 和 $\operatorname{col}$ 表示当前所在的行和列，最后 $\operatorname{row}$ 的位置即为**非自由元的个数**。

由于遇到所有自由元时，我们都保持行不变，所以可以理解为：我们把所有自由元都挪到了最后面没有被 $\operatorname{row}$ 遍历到的几行当中去。

> 这可以结合上面 $5\times 5$ 的例子进行理解。
>
> 在遇到自由元 $x_3$ 后，跳过第 $3$ 列，保持 $\operatorname{row}$ 不变。所以第 $3$ 行保留的主元是 $x_4$，同理第 $4$ 行保留的主元是 $x_5$.
>
> 所以第 $5$ 行即代表第 $x_3$，第 $5$ 行的方程为：$0\times x_3=c_5$.

## 代码

下面代码会将所有自由元保留到最后的连续几行。

因此，要判断解的情况，只需要去判断第 $\operatorname{row+1}\sim n$ 行的系数 $c$ 是否为 $0$ 即可。

```cpp
//高斯消元法，返回-1为无解，0为无穷解，1为唯一解
double A[MAXN][MAXN], ans[MAXN];
int Gauss() {
	int row = 1, max_row;
	for(int col = 1; col <= m; col++) {
		max_row = row;
		for(int i = row + 1; i <= n; i++) { //提高精度 找到一列中值最大的
			if(fabs(A[i][col]) > fabs(A[max_row][col])) max_row = i;
		}
		if(fabs(A[max_row][col]) < eps)
			continue; //如果col是自由元，那么跳过这一列，但行不变
		if(max_row != row) { //swap(当前行，最大值所在的行)
			for(int i = 1; i <= m + 1; i++) swap(A[row][i], A[max_row][i]);
		}
		for(int i = row + 1; i <= n; i++) {
			double t = A[i][col] / A[row][col];
			for(int j = 1; j <= m + 1; j++) {
				A[i][j] -= A[row][j] * t;
			}
		}
		row++;
	}
	row--;
	if(row < n) {
		for(int i = row + 1; i <= n; i++) {
			//如果左边所有系数都是0，等号右边不是0，则无解
			if(abs(A[i][m + 1]) > eps) return -1;
		} //否则有无数解
		return 0;
	}
	for(int i = m; i >= 1; i--) { //回代
		for(int j = i + 1; j <= m; j++) A[i][m + 1] -= A[i][j] * ans[j];
		ans[i] = A[i][m + 1] / A[i][i];
	}
	return 1;
}
```

# 高斯-约旦消元法

> 高斯-约旦消元法，是高斯消元法的另一个版本，相异之处就是这算法产生出来的矩阵是一个**简化行梯阵式**，**省去了回代过程**。
>
> 相比起高斯消元法，此算法的效率和精度都比较低，但代码难度也低。

## 算法思想

构造方程组矩阵的方法同上，化简后的矩阵形如：
$$
\left[\begin{matrix}
a_{1,1} &0        & 0      &\cdots & 0     & 0 \\
0       & a_{2,2} & 0      &\cdots & 0     & 0 \\
0       & 0       &a_{3,3} &\cdots & 0     & 0 \\
\cdots  &\cdots   &\cdots  &\cdots &\cdots & \cdots \\
0       & 0       &0       &\cdots & 0     & a_{n,m}
\end{matrix} \middle|
\begin{matrix}
c_{1} \\
c_{2} \\
c_{3} \\
\cdots\\
c_{n}
\end{matrix} \right]
$$
**只保留主对角线上的元素**。

显然，这样就省去了回代的步骤，直接 $a_{i, i}\cdot x_i=c_i$ 就可以解出所有未知数了。

## 算法实现

在上面的[高斯消元法](#高斯消元法)中，有这样一句话：

> 当我们枚举到第 $c$ 列的时候，我们要让 $\sum_{i=c+1}^{n}[a_{i, c}\neq0]=0$.

要想让最终的矩阵只保留主对角线上的元素，我们只需要把这句话改为：

当我们枚举到第 $c$ 列的时候，我们要让 $\sum_{i\neq c}[a_{i, c}\neq0]=0$，也就是**直接消掉其他所有行**。

```cpp
double A[MAXN][MAXN], ans[MAXN];
void Gauss_Jordan() {
	for(int c = 1; c <= n; c++) {
		for(int i = c + 1; i <= n; i++) {
			double t = A[i][c] / A[c][c];
			for(int j = c + 1; j <= n + 1; j++) {
				A[i][j] -= A[c][j] * t;
			}
		}
	}
}
```

## 扩大适用范围

在高斯-约旦消元法中，我们同样可以选择每列最大的数作为主元，以提高精度。

同时，我们可以用类似的方法判断方程组解的个数。

两种算法的区别只在第 $\rm{15\sim16}$ 行、第 $33$ 行这两处。

```cpp
//高斯-约旦消元法，返回-1为无解，0为无穷解，1为唯一解
double A[MAXN][MAXN], ans[MAXN];
int Gauss_Jordan() {
	int row = 1, max_row;
	for(int col = 1; col <= m; col++) {
		max_row = row;
		for(int i = row + 1; i <= n; i++) {
			if(fabs(A[i][col]) > fabs(A[max_row][col])) max_row = i;
		}
		if(fabs(A[max_row][col]) < eps)
			continue; //如果col是自由元，那么跳过这一列，但行不变
		if(max_row != row) { //swap(当前行，最大值所在的行)
			for(int i = 1; i <= n + 1; i++) swap(A[row][i], A[max_row][i]);
		}
		for(int i = 1; i <= n; i++) {
			if(i == row) continue; //每列只保留第row行的元素
			double t = A[i][col] / A[row][col];
			for(int j = 1; j <= m + 1; j++) {
				A[i][j] -= A[row][j] * t;
			}
		}
		row++;
	}
	row--;
	if(row < n) {
		for(int i = row + 1; i <= n; i++) {
			//如果左边所有系数都是0，等号右边不是0，则无解
			if(fabs(A[i][n + 1]) > eps) return -1;
		} //否则有无数解
		return 0;
	}
	for(int i = 1; i <= m; i++) {
		ans[i] = A[i][m + 1] / A[i][i];
	}
	return 1;
}
```

# 解异或方程组

对于异或方程组，高斯消元法同样适用。

在实数方程组中，我们消掉第 $v$ 个变元的方法是：用第 $a$ 行减去第 $b$ 行的 $k$ 倍，使第 $a$ 行第 $v$ 列变为 $0$。

在异或方程组中，若第 $a$ 行第 $v$ 列的值为 $1$，就**将第 $a$ 行异或上第 $b$ 行**。（按列分别异或）

由于 $x\in\{0, 1\}$，所以 $(a\cdot x)\oplus (b\cdot x)=(a\oplus b)\cdot x$，因此第 $a$ 行异或后的方程是成立的。

注意到异或方程组的增广矩阵是 $01$ 矩阵（矩阵中仅含有 $0$ 与 $1$），所以我们可以使用 C++ 中的 `std::bitset` 进行优化，将时间复杂度降为 $O(\dfrac{n^2m}{\omega})$，其中 $n$ 为元的个数，$m$ 为方程条数，$\omega$ 一般为 $32$（与机器有关）。

## 代码

```cpp
bitset<MAXN> A[MAXM];
int Gauss() {//A[][]是m行n列的矩阵
	for(int col = 1; col <= n; col++) {
		int x = col;
		while(x <= m && !A[x][col]) x++;
		if(x == m + 1) return 0;
		ans = max(ans, x);
		if(col != x) {//swap(A[x], A[col]);
			bitset<MAXN> tmp = A[x];
			A[x] = A[col]; A[col] = tmp;
		}
		for(int i = 1; i <= m; i++) {
			if(i != col && A[i][col]) A[i] ^= A[col];
		}
	}
	return 1;
}
```



# 模意义下的高斯消元 - 模数为质数

> 对于模数为合数的情况，会在后面提到。

##  分析

注意到，在实数域内的高斯-约旦消元有这样一段代码：

只要**把除法操作改为乘逆元**即可。

## 代码

普通的高斯-约旦消元：

```cpp
//......
for(int i = 1; i <= n; i++) {
    if(i == row) continue;
    double t = A[i][col] / A[row][col];
    for(int j = 1; j <= m + 1; j++) {
        A[i][j] -= A[row][j] * t;
    }
}
//......
for(int i = 1; i <= n; i++) {
    ans[i] = A[i][n + 1] / A[i][i];
}
```

**模意义下的**高斯-约旦消元：

```cpp
//......
for(int i = 1; i <= n; i++) {
	if(i == row) continue;
	int k = A[i][col] * inv(a[col][col]) % mod;
	for(int j = 1; j <= m + 1; j++) {
		A[i][j] = (A[i][j] + A[col][j] * (mod - k)) % mod;
	}
}
//......
for(int i = 1; i <= n; i++) {
    ans[i] = A[i][n + 1] * inv(A[i][i]) % mod;
}
```

# 模意义下的高斯消元 - 模数为合数

> 模数为合数时，有些数没有逆元，需要另寻方法。

## 分析

对于模数为合数的情况，我们使用类似**辗转相除**的方法。

对于两行 $a, b$，现在要消去变元 $c$。

在辗转相除法时，我们会用 $a\leftarrow a\bmod b$，但现在要同时操作一整行，没法直接取模，所以要模拟一下取模的过程。

我们每次将 $a$ 减去 $b$ 的尽可能多倍（$k\in \Bbb{Z}^{+}$），使仍然保持 $a_c\geq 0$，之后交换 $a, b$，重复操作，直到 $a_c=0$.

具体地，所减去的倍数 $k=\lfloor\dfrac{a_c}{b_c}\rfloor$.

##  代码

辗转相除法就不用每次找最大的换上去了，因为不需要控制精度，都是整数。

```cpp
void Gauss() {
	for(int col = 1; col <= n; col++) {
		for(int row = col + 1; row <= n; row++) {
			while(a[row][col]) {
				int t = a[col][col] / a[row][col];
				for(int i = 1; i <= n; i++) {
					a[col][i] = (a[col][i] + a[row][i] * (p - t)) % p;
				}
				for(int i = 1; i <= n; i++) swap(a[col][i], a[row][i]);
			}
		}
	}
}
```

# [JSOI2008] 球形空间产生器

## 题意

给定一个 $n$ 维超球上的 $n+1$ 个点的坐标，求球心坐标。保证有唯一解。

$n<=10$，坐标绝对值为 $20000$ 以内实数。

> 球心：到球面任意一点距离都相等的点。
>
> 距离：若两点坐标为 $A(a_1, a_2\cdots, a_n)$，$B(b_1, b_2, \cdots, b_n)$，那么：
> $$
> \operatorname{dis(A, B)}=\sqrt{(a_1-b_1)^2+(a_2-b_2)^2+\cdots+(a_n-b_n)^2}
> $$

## 分析

设圆心坐标为 $(x_1, x_2, \cdots, x_n)$，那么容易列出一个方程组：
$$
\left\{
\begin{array}{}
\sum_{i=1}^n(a_{1, i}-x_i)^2=C^2 &(1)\\
\sum_{i=1}^n(a_{2, i}-x_i)^2=C^2 &(2)\\
\cdots&\cdots\\
\sum_{i=1}^n(a_{n, i}-x_i)^2=C^2&(n)\\
\sum_{i=1}^n(a_{n+1, i}-x_i)^2=C^2&(n+1)\\
\end{array}
\right.
$$
其中 $C$ 就代表 $n$ 维超球的半径。

但这不是一个线性方程组，没法快速求解。

又注意到，求出圆心坐标，只有 $n$ 个未知数，而现在能列出 $n+1$ 个方程，所以考虑进行**差分**。

## 差分

对于其中的第 $p$ 个方程，我们对其进行整理，变为：
$$
\sum_{i=1}^{n}{{a_{p, i}}^2}+\sum_{i=1}^{n}{{x_i}^2}-\sum_{i=1}^{n}{2\cdot a_{p, i}\cdot x_i} \tag{p}
$$
同理有：
$$
\sum_{i=1}^{n}{{a_{p+1, i}}^2}+\sum_{i=1}^{n}{{x_i}^2}-\sum_{i=1}^{n}{2\cdot a_{p+1, i}\cdot x_i} \tag{p+1}
$$
我们用 $(p+1)-(p)$，得到：
$$
\sum_{i=1}^{n}{2(a_{p, i}-a_{p+1, i})x_i}=\sum_{i=1}^{n}{({a_{p, i}}^2-{a_{p+1, i}}^2)} \tag{*}
$$
以此类推，我们能得到 $n$ 个形如 $(*)$ 的线性方程，于是问题可以用高斯消元求解。

最终的系数矩阵为：
$$
\left[\begin{matrix}
2(a_{1, 1}-a_{2, 1}) & 2(a_{1, 2}-a_{2, 2}) & \cdots  & 2(a_{1, n}-a_{2, n}) \\
2(a_{2, 1}-a_{3, 1}) & 2(a_{2, 2}-a_{3, 2}) & \cdots  & 2(a_{2, n}-a_{3, n}) \\
\cdots               & \cdots               & \ddots  & \cdots               \\
2(a_{n, 1}-a_{n+1, 1}) & 2(a_{n, 2}-a_{n+1, 2}) & \cdots & 2(a_{n, n}-a_{n+1, n})
\end{matrix} \middle|
\begin{matrix}
\sum_{j=1}^n({a_{1, j}}^2-{a_{2, j}}^2) \\
\sum_{j=1}^n({a_{2, j}}^2-{a_{3, j}}^2) \\
\vdots\\
\sum_{j=1}^n({a_{n, j}}^2-{a_{n+1, j}}^2)
\end{matrix} \right]
$$

## 代码

[Ubuntu Pastebin](https://paste.ubuntu.com/p/DdF9c7gjcg/)，应该不用看吧？

# [SDOI2010] 外星千足虫

## 题意

> 给定 $n$ 个点，按点权的奇偶性分为两类。
>
> 给定 $m$ 次统计数据，每条数据包含：若干个**被选中点**，以及**被选中的点权和的奇偶性**。
>
> 请判断每个点权的奇偶性。
>
> 题目保证有解，若解不唯一，输出 $\rm{Cannot\;Determine}$.
>

## 分析

设 $a_{i, j}\in\{0, 1\}$ 表示第 $i$ 条数据中，第 $j$ 个点是否被选中， $b_i\in \{0, 1\}$ 表示第 $i$ 次统计结果的奇偶性。

设 $x_i=w_i\bmod 2$ 表示第 $i$ 个点权的奇偶性。

那么有同余方程组：
$$
\left\{
\begin{array}{}
a_{1, 1}x_1+a_{1, 2}x_2+\cdots+a_{1, n}x_n=b_1\pmod{2} &&(1)\\
a_{2, 1}x_1+a_{2, 2}x_2+\cdots+a_{2, n}x_n=b_2\pmod{2} &&(2)\\
\cdots                                                 &&\cdots\\
a_{m, 1}x_1+a_{m, 2}x_2+\cdots+a_{m, n}x_n=b_m\pmod{2}&&(m)\\
\end{array}
\right.
$$
注意到，方程左右两侧都**只需要考虑二进制下的第一位**，因此可以转化为一个异或方程组：
$$
\left\{
\begin{array}{}
a_{1, 1}x_1\oplus a_{1, 2}x_2\oplus \cdots \oplus a_{1, n}x_n=b_1 &&(1)\\
a_{2, 1}x_1\oplus a_{2, 2}x_2\oplus \cdots \oplus a_{2, n}x_n=b_2 &&(2)\\
\cdots                                                            &&\cdots\\
a_{m, 1}x_1\oplus a_{m, 2}x_2\oplus \cdots \oplus a_{m, n}x_n=b_m &&(m)\\
\end{array}
\right.
$$
解异或方程组即可。

## 其他要求

> 题目要求：输出一个不超过 $M$ 的正整数 $K$，表明在第 $K$ 次统计结束后就可以确定唯一解。

对于这个要求，我们只需要在高斯消元时每次对行取 $\max$ 即可，详见下面代码的第 $7$ 行。

在下面代码的消元过程中，对于第 $\rm{col}$ 列，我们会贪心地找到编号最小的 $x$ 满足 $A[x][col]=1$.

所以，要想确定方程的解，至少要通过前 $x$ 条统计数据，否则第 $\rm{col}$ 个变量就无法求解。

# Band-Matrix

## 简介

这是个在 OI 中很经典的 trick。

Band-Matrix，带状矩阵，对这种矩阵进行**高斯消元**时，可以进行优化。

顾名思义，带状矩阵是长条形的，差不多是这样：

<img src="https://cdn.tonyyin.top/2021/10/14/d0573d1ba3f5a.png" style="zoom:50%;" />

白色部分为 $0$，红色部分有数，其它部分先不管。

这样中间形成了一个宽度为 $d$ 的带，这样消元的复杂度可以降到 $\mathcal{O}(nd^2)$.

常见于一些期望DP的问题。

## 消元流程

在图中，绿色节点是我们要保留的主元。

注意到在这种形式下，对于任意的 $A_{i, i}$，其向下、向右的**有效数字个数均 $\leq d-1$**.

所以假设现在要消第 $i $ 列，那么从第 $i$ 行开始往下枚举 $d-1$ 行，每行往右消 $d$ 个数字，最后能得到一个倒三角矩阵。

之后回代。回代过程的复杂度最高是 $\mathcal{O}(n^2)$ 的，不是复杂度瓶颈，但也可以根据带状矩阵的形状优化。

### 判断无解/存在自由元

大部分题好像都有解，下面的参考资料中也有讲。

### 细节

如果 $n$ 太大，二维数组开不出来，可以只记录带状矩阵的部分，也可以用 `unordered_map`.

## 参考资料

[浅谈高斯消元拓展之 band-matrix - Froggy 珂学家的博客](https://www.luogu.com.cn/blog/froggy/qian-tan-gao-si-xiao-yuan-ta-zhan-zhi-band-matrix)

# P5516 [MtOI2019]小铃的烦恼

## 题意

给定长度为 $n$ 的序列，第 $i$ 个元素为 $a_i$.

每次随机取出一个有序数对 $(x, y)(x\neq y)$，然后以 $s_{x, y}$ 的成功概率将 $a_x$ 变为 $a_y$（若不成功则不变）。

要让全部元素都相同，求期望次数。

$1\leq a_i \leq26$，$n\leq 2\times 10^3$.

对于所有数据，满足 $(\sum\limits_{a=1}^{n}\sum\limits_{b=1}^n s_{a, b})=n^2$.

## 分析

高斯消元？看不出来有方程。

由于 $(\sum\limits_{a=1}^{n}\sum\limits_{b=1}^n s_{a, b})=n^2$ ，显然每个 $s_{i, j}$ 都恒为 $1$。这表明对于选出的任意两个数 $x, y$，**每次操作都能成功**。

然而这并没有使这题与概率无关，因为 $x, y$ 的选取仍然是随机的。

考虑到最后全部的元素会相同，但不知道都变成什么元素，因此我们先枚举目标元素**（最后剩下的元素）。

下面开始推式子。

## 概率

对序列的每次操作，可能会使目标元素的个数增加：$-1/0/1$ 个。

可以发现，**一种元素最后成为目标元素的概率，取决于一开始序列中有多少个这种元素**。

> 因此，下面的分析建立在已经确定目标元素的基础上，换言之，只关注有多少个目标元素。

设 $p[i]$ 表示：**此时有 $i$ 个目标元素，全部元素都变成当前选取的目标元素的概率。**

那么：
$$
p[i]=\frac{i(n-i)}{n(n-1)}p[i-1]+\frac{i(n-i)}{n(n-1)}p[i+1]+(1-2\frac{i(n-i)}{n(n-1)})p[i]
$$
方程代表：从有 $i$ 个目标元素的状态为基础，**操作一次**，产生的三种可能情况。

要让目标元素个数 $\pm 1$，就要从 $i$ 个数中选一个，再从其他的 $n-i$ 个数中选一个。

从 $n$ 个数中随机选取两个的方案数是 $n(n-1)$，因此目标元素个数 $\pm 1$ 的概率均为 $\frac{i(n-i)}{n(n-1)}$.

而目标元素个数不变的概率，就是 $1$ 减去前两种情况剩下的概率。

对方程进行整理，设 $k=\frac{i(n-1)}{n(n-1)}$，那么有：
$$
\begin{aligned}
p[i] &= k\cdot p[i-1]+k\cdot p[i+1]+(1-2k)p[i]\\
2k\cdot p[i] &= k\cdot p[i-1]+k\cdot p[i+1]\\
\end{aligned}
$$
把 $k$ 约掉：
$$
2p[i]=p[i-1]+p[i+1]
$$
也就是：
$$
p[i+1]-p[i]=p[i]-p[i-1]
$$
这说明 $p$ 的相邻两项差值相同，**$p$ 是等差数列**。

又因为显然， $p[0]=0$，$p[n]=1$，可以得到一个非常简单的形式：
$$
p[i]=\frac{i}{n}
$$
在此基础上，我们继续研究期望。

## 期望

期望的分析要简单一些，设 $f[i]$ 表示：**当前有 $i$ 个目标元素，使全部元素都变成目标元素的期望步数**。

我们只关心能使目标元素个数**变化**的操作。可以发现，目标元素变化（$+1/-1$）的概率为 $2\frac{i(n-i)}{n(n-1)}$.

因此，操作一次，期望变化 $\frac{2i(n-i)}{n(n-1)}$；变化为 $1$，期望次数就是 $\frac{n(n-1)}{2i(n-i)}$.

所以得到和期望相关的方程：
$$
p[i]f[i]=p[i]\cdot\frac{n(n-1)}{2i(n - i)} + \frac{1}{2}p[i - 1]f[i - 1]+\frac{1}{2}p[i + 1]f[i + 1]
$$
方程整体的含义是：当前状态下有 $i$ 个目标元素，要让个数变化 $1$。第一项就是变化 $1$ 的**期望操作次数**，后两项分别是个数 $+1/-1$ 后的情况。

$f[i]$ 乘上了 $p[i]$，因为 $p[i]$ 的概率最终能移动到目标元素，$f[i]$ 的期望贡献只有 $p[i]\times f[i]$。

由于 $+1/-1$ 的概率相同，所以两种情况的系数都是 $\frac{1}{2}$.

同理，期望操作也要乘上 $p[i]$，因为只有 $p[i]$ 的概率，会向目标方向转移。（或者也可以把左边的 $p[i]$ 除过去，用另一种方法理解）

把 $p[i]=\frac{i}{n}$ 代入，整理得到：
$$
\begin{aligned}
f[i] &= \frac{n(n-1)}{2i(n-i)}+\frac{i-1}{2i}f[i-1]+\frac{i+1}{2i}f[i+1]\\
f[i]-\frac{i-1}{2i}f[i-1]-\frac{i+1}{2i}f[i+1]&= \frac{n(n-1)}{2i(n-i)}
\end{aligned}
$$
又因为 $f[n]=0$， $i\in[1, n-1]$.

所以现在得到了 $n-1$ 个方程，$n-1$ 个未知数，可以高斯消元解方程组。

然而本题数据范围 $n\leq 2\times 10^3$，$\mathcal{O}(n^3)$ 的高斯消元不可行，下面是一些消元技巧。

## 高斯消元

每个方程只有三项是有系数的，矩阵长这样：
$$
\begin{matrix}
&&\textcolor{gray}{0} &\textcolor{gray}{1} &\textcolor{gray}{2} &\textcolor{gray}{3} &\textcolor{gray}{4}\\
\textcolor{white}{1}\\
\textcolor{gray}{1} &&0 &1 &a &0 &0\\
\textcolor{gray}{2} &&0 &b &1 &c &0\\
\textcolor{gray}{3} &&0 &0 &d &1 &f\\
\textcolor{gray}{4} &&0 &0 &0 &g &1
\end{matrix}
$$
可以发现，对第 $i$ 个元，只有$i-1$，$i$ 和 $i+1$ 行的系数非零。所以如果用高斯-约旦消元法，只需要消第 $i-1$ 和 $i+1$ 行的就行了。

又因为每行只有三个变元的系数和一个等号右边的系数，所以这样的时间复杂度是 $\mathcal{O}(n)$.

此外，空间复杂度也能比较轻易地优化到 $\mathcal{O}(n)$，可以参考其他题解。

## 代码

[Ubuntu Pastebin](https://paste.ubuntu.com/p/w57MWzy4hV/)

概率都是 $1.00$，可以不用输入。注意数组要用 `double`。

## 参考资料

[a_forever_dream的博客 - CSDN博客](https://blog.csdn.net/a_forever_dream/article/details/105512284)

# P3232 - [HNOI2013]游走

## 题意

给定一个 $n$ 个点 $m$ 条边的**无向连通图**，顶点从 $1$ 编号到 $n$，边从 $1$ 编号到 $m$。

小 Z 在该图上进行随机游走，初始时小 Z 在 $1$ 号顶点，每一步小 Z 以**相等**的概率随机选择当前顶点的某条边，沿着这条边走到下一个顶点，获得等于这条**边的编号**的分数。当小 Z 到达 $n$ 号顶点时游走结束，总分为所有获得的分数之和。

现在，请你对这 $m$ 条边进行**编号**，使得小 Z 获得的总分的期望值**最小**。

求最小期望值。

$2\leq n\leq 500$，$1\leq m\leq 125000$，无重边无自环。

## 分析

设边 $(i,j)$ 的期望经过次数为 $E_{(i, j)}$，答案即 $\sum E_{(i, j)}  v_{(i,  j)}$，因为每条边的边权 $v$ 可以自己定，容易想到贪心。

先算出期望经过次数，然后**让经过次数最大的边的编号最小**。就是一个期望DP。

## 期望DP

由于 $m$ 比较大，并且我们不会列出只和 $E$ 相关的方程式，所以先处理点。

**设 $f_i$ 表示从 $1$ 走到 $n$ 的过程中，经过点 $i$ 的期望次数。** 用其表示 $E$，
$$
E_{(i,j)}=[i\neq n]\frac{f_i}{\operatorname{deg_i}} + [j\neq n]\frac{f_j}{\operatorname{deg}_j}
$$
经过一条边有两种可能的方向，分别考虑从 $i\rightarrow j$ 和 $j\rightarrow i$。

首先根据题意要求起点不能为 $n$，从 $i$ 出发，下一次恰好走到 $j$ 的概率即为 $\dfrac{1}{\operatorname{deg}_i}$。

下面计算 $f_i$，和上面类似地，有：
$$
f_i=
\left\{
\begin{array}{lr}
\sum_{(i, j)\in E,j\neq n}\frac{f_j}{\operatorname{deg}_j}+1, &i = 1\\
\sum_{(i, j)\in E,j\neq n}\frac{f_j}{\operatorname{deg}_j},   &i\neq1\\
\end{array}
\right.
$$
相当于对于每个 $i$，枚举上一个经过的点 $j$，从 $j$ 走到 $i$ 的概率为 $\dfrac{1}{\operatorname{deg}_j}$.

对于节点 $1$，初始时一定会经过一次，所以 $f_i$ 要多 $1$.

对于节点 $n$，走到之后就不能出去了，所以枚举 $j$ 时 $j\neq n$.

于是我们得到了 $n$ 个有关 $f$ 的方程，转移有后效性，用高斯消元求解即可。

时间复杂度：$\mathcal{O}(n^3)$.

## 代码

[Ubuntu Pastebin](https://paste.ubuntu.com/p/w57MWzy4hV/)

## 模型

这种模型在概率期望题中很常见，只不过表达形式有所不同。

给定若干个点，点 $u$ 有 $P_{u, v}$ 的概率通向点 $v$。终点或某些其他点，进去之后就不能出来。

求从点 $S$ 出发，走到点 $T$ 的期望步数。

这个图如果不是 $\rm{DAG}$ 的话就不能用 DP 解决，只能列方程，用高斯消元。

设 $E_i$ 表示从 $i$ 走到 $T$ 的期望步数，方程形如：
$$
E_i=
\left\{
\begin{array}{lr}
\sum_{j=1}^n E_jP_{i, j} + 1, &i\neq T\\
0, &i=T
\end{array}
\right.
$$
就是所有后继状态的期望步数乘上对应转移概率，再求和。

这道题是算期望经过次数，不是步数，所以转移略有不同。后面的题大多都是这种模型。

## 参考资料

[治疗之雨题解 - shadowice1984 的博客](https://www.luogu.com.cn/blog/ShadowassIIXVIIIIV/solution-p4457)

# CF24D - Broken robot

## 题意

$n$ 行 $m$ 列的矩阵，$(1,1)$ 是左上角，$(n,m)$ 是右下角。

初始状态在 $(x,y)$。每次等概率向**左**，**右**，**下**走或**原地不动**，但不能走出去。

**等概率：**若其位于第一列，只能向右、下走或原地不动，则选择这三种的概率均为 $\frac{1}{3}$.

求走到最后一行期望的步数，保留四位小数。

$1\leq n, m\leq 10^3$.

## 分析

令 $f_{i,j}$ 表示从位置 $(i,j)$ 走到最后一行，所需行动次数的期望值。目标是要求出 $f_{x, y}$.

首先，初始状态为：$\forall j\in[1,m],f_{n,j}=0$。

不在最后一行时：
$$
f_{i, j}=
\left\{
\begin{array}{lc}
\frac{1}{3}(f_{i,j}+f_{i,j+1}+f_{i+1,j})+1, &j=1\\
\frac{1}{4}(f_{i,j}+f_{i,j-1}+f_{i,j+1}+f_{i+1,j})+1, &1 < j < m\\
\frac{1}{3}(f_{i,j}+f_{i,j-1}+f_{i+1,j})+1, &j=m\\
\end{array}
\right.
$$
在第一列不能再向左走，在第 $m$ 列不能再向右走。

暴力高斯消元，时间复杂度 $\mathcal{O}(n^6)$，不能接受。空间复杂度 $\mathcal{O}(n^4)$，更不能接受。

## 消元

显然第 $i$ 行的状态只与 $i$ 和 $i+1$ 行有关，在行维度上，其满足无后效性。我们还知道 $f_n=0$，所以**从下往上逐行计算**。

于是方程中 $f_{i+1,j}$ 是已知的常数，直接移到等式右侧，得到方程组：
$$
\left\{\begin{array}{cc}
-\frac{2}{3}f_{i,j} + \frac{1}{3}f_{i,j+1} = -\frac{1}{3}f_{i+1,j}-1, &j=1\\
-\frac{3}{4}f_{i,j} + \frac{1}{4}f_{i,j-1} + \frac{1}{4}f_{i,j+1} = -\frac{1}{4}f_{i+1,j} - 1, &1<j<m\\
-\frac{2}{3}f_{i,j} + \frac{1}{3}f_{i,j-1} = -\frac{1}{3}f_{i+1,j}-1, &j=m\\
\end{array}\right.
$$
这样的复杂度变为 $\mathcal{O}(n^4)$。

再观察，其系数矩阵只有主对角线附近有数，其余部分均为 $0$，比如 $m=5$ 时：
$$
\begin{bmatrix}
-\frac{2}{3} & \frac{1}{3} & 0 & 0 & 0\\
\frac{1}{4} & -\frac{3}{4} & \frac{1}{4} & 0 & 0\\
0 & \frac{1}{4} & -\frac{3}{4} & \frac{1}{4} & 0\\
0 & 0 & \frac{1}{4} & -\frac{3}{4} & \frac{1}{4}\\
0 & 0 & 0 & \frac{1}{3} & -\frac{2}{3}\\
\end{bmatrix}=\begin{bmatrix}
-\frac{1}{3}f_{i+1,j}-1\\
-\frac{1}{4}f_{i+1,j}-1\\
-\frac{1}{4}f_{i+1,j}-1\\
-\frac{1}{4}f_{i+1,j}-1\\
-\frac{1}{3}f_{i+1,j}-1\\
\end{bmatrix}
$$
可以用 $\rm{Band-Matrix}$ 继续优化。

这道题中，带的宽度可以看成常数。时间复杂度为 $\mathcal{O}(nm)$，空间复杂度为 $\mathcal{O}(m^2)$ 或 $\mathcal{O}(m)$.

## 代码

我的代码只保留了每行有用的三列：[Ubuntu Pastebin](https://paste.ubuntu.com/p/Gr6z68DQFs/)，下面参考资料中的代码保留了整个矩阵。

## 参考资料

[CF24D Broken robot - maoyiting's blog - 洛谷博客](https://www.luogu.com.cn/blog/maoyiting/CF24D)

# CF963E - Circles of Waiting

## 题意

在平面直角坐标系上，有一个神奇的点，一开始在 $(0, 0)$ 。

每秒钟这个点都会随机移动，如果它目前在 $(x, y)$，那么下一秒它：

- 在 $(x - 1, y)$ 的概率是 $p_1$，
- 在 $(x, y - 1)$ 的概率是 $p_2$，
- 在 $(x + 1, y)$ 的概率是 $p_3$，
- 在 $(x, y + 1)$ 的概率是 $p_4$.

保证 $p_1 + p_2 + p_3 + p_4 = 1$，各次移动互不关联。

求出这个点移动到**距离原点距离大于 $R$ 的点**的期望步数，对 $10^9+7$ 取模。

欧几里得距离，$0\leq R \leq 50$。

## 分析

先理解一下题意，下面是 [CF官方题解](https://codeforces.com/blog/entry/58991) 里的图。

![](https://cdn.tonyyin.top/2022/03/05/dbe9738850193.png)

只能在整点上移动，移出半径为 $R$ 的圆为止。

期望DP，设 $f_{x, y}$ 为：从 $(x,y)$ 走出圆所需要的期望步数，方程显然：
$$
f_{x, y}=
\left\{
\begin{array}{lc}
f_{x-1, y}p_1+f_{x, y-1}p_2+f_{x+1, y}p_3+f_{x, y+1}p_4+1, &\sqrt{x^2+y^2}\leq R\\
0, &\sqrt{x^2+y^2}> R
\end{array}
\right.
$$
高斯消元直接解的话，点数是略小于 $4R^2$，时间复杂度是 $\mathcal{O}(R^6)$.

考虑利用 $\rm{Band-Matrix}$ 优化。

![](https://cdn.tonyyin.top/2021/10/15/7dea4ab09cdd0.png)

如果我们对圆内的点，从左到右，从上到下进行编号。若 $\operatorname{id}(x, y)=x$，则其四周点的 $\operatorname{id}\in[x-2R, x+2R]$.

因此，第 $x$ 列，非零的范围是 $[x-2R, x+2R]$，矩阵是宽度为 $4R$ 的带状矩阵，

消元时只需要考虑向下的 $2R$ 列和向右的 $2R$ 列，消成倒三角矩阵，再回代即可。

这样的时间复杂度是 $\mathcal{O}(R^4)$ 的。未知数个数是 $R^2$ 级别的，消元是 $R\times R$ 级别的。

## 代码

代码实现上，注意坐标可能为负，细节比较多。

[Ubuntu Pastebin](https://paste.ubuntu.com/p/KXxbpv5Ndr/)

## 参考资料

这篇比洛谷题解详细：[UnnamedOrange - CSDN博客](https://blog.csdn.net/lycheng1215/article/details/80180178)

# 线性代数入门

这部分差的有点多，赛后再补。

## 线性变换与矩阵

[线性代数的本质 - 03 - 矩阵与线性变换](https://www.bilibili.com/video/BV1ys411472E?p=4)

[线性代数的本质 - 04 - 矩阵乘法与线性变换复合](https://www.bilibili.com/video/BV1ys411472E?p=4)

# 矩阵求逆

> 求一个 $N\times N$ 的矩阵的逆矩阵。答案对 ${10}^9+7$ 取模。

## 算法思想

从线性变换的角度理解矩阵和逆矩阵。

逆矩阵表示：把原矩阵变到单位矩阵这一线性变换过程。因此，我们通过高斯消元把原矩阵变换为单位矩阵，同时新建一个矩阵记录变换过程。

## 算法流程

设 $A$ 是原矩阵，$A^{-1}$ 为 $A$ 的逆矩阵，则 $A\cdot A^{-1}=E_n$，$E_n$ 代表 $n$ 阶单位矩阵。
$$
E=\begin{bmatrix}
1      &0      &\cdots &0      &0      \\
0      &1      &\cdots &0      &0      \\
\cdots &\cdots &1      &\cdots &\cdots \\
0      &0      &\cdots &1      &0      \\
0      &0      &\cdots &0      &1
\end{bmatrix}
$$
若 我们**把一个单位矩阵 $E_n$ 并在 $A$ 的右边**，之后通过高斯-约旦消元法，把 $A$ 消元成 $E_n$ 的形式，此时 $B$ 就是矩阵 $A$ 的逆。

**具体地，**以 $n=5$ 为例，把两个矩阵合到一起：
$$
\begin{bmatrix}
A & E_n
\end{bmatrix}=
\left[
\begin{matrix}
a_{1, 1} &a_{1, 2} &a_{1, 3} &a_{1, 4} &a_{1, 5}    \\
a_{2, 1} &a_{2, 2} &a_{2, 3} &a_{2, 4} &a_{2, 5}    \\
a_{3, 1} &a_{3, 2} &a_{3, 3} &a_{3, 4} &a_{3, 5}    \\
a_{4, 1} &a_{4, 2} &a_{4, 3} &a_{4, 4} &a_{4, 5}    \\
a_{5, 1} &a_{5, 2} &a_{5, 3} &a_{5, 4} &a_{5, 5}
\end{matrix}
\middle|
\begin{matrix}
1 &  &  &  &    \\
  &1 &  &  &    \\
  &  &1 &  &    \\
  &  &  &1 &    \\
  &  &  &  &1
\end{matrix}
\right]
$$
通过高斯消元，把左半部分矩阵变成单位矩阵：
$$
\begin{bmatrix}
E_n & A^{-1}
\end{bmatrix}=
\left[
\begin{matrix}
1 &  &  &  &    \\
  &1 &  &  &    \\
  &  &1 &  &    \\
  &  &  &1 &    \\
  &  &  &  &1
\end{matrix}
\middle|
\begin{matrix}
b_{1, 1} &b_{1, 2} &b_{1, 3} &b_{1, 4} &b_{1, 5}    \\
b_{2, 1} &b_{2, 2} &b_{2, 3} &b_{2, 4} &b_{2, 5}    \\
b_{3, 1} &b_{3, 2} &b_{3, 3} &b_{3, 4} &b_{3, 5}    \\
b_{4, 1} &b_{4, 2} &b_{4, 3} &b_{4, 4} &b_{4, 5}    \\
b_{5, 1} &b_{5, 2} &b_{5, 3} &b_{5, 4} &b_{5, 5}
\end{matrix}
\right]
$$
此时矩阵 $B=A^{-1}$.

# 行列式

> 给定一个 $n$ 阶行列式 $A$，求 $|A|$。结果对 $p$ 取模。

## 行列式

对于一个 $n$ 阶矩阵，有：

$\operatorname{det}A=\sum\limits_{p}(-1)^{\tau(p)}\prod\limits_{i=1}^na_{i,p_i} $，其中 $p$ 是任意一个排列，$\tau(p)$ 指的是 $p$ 中的逆序对数。但这个定义暂时没啥用。

矩阵 $A$ 的行列式可以表达为 $|A|$.

## 性质

建议先看 3b1b 的视频：[线性代数的本质 - 05 - 行列式](https://www.bilibili.com/video/BV1ys411472E?p=7)，再看下面的性质。

以下内容参考 [MIT公开课 - 行列式笔记](https://cdn.tonyyin.top/2021/10/16/6ccbfd6c0b74e.pdf) 和 [线性代数公开课 - MIT 18.06 Linear Algebra - 系列合集 - 哔哩哔哩](https://www.bilibili.com/video/BV1zx411g7gq).

#### 性质 $1$

$|E|=1$，$E$ 为单位矩阵。直接用代数定义就可以证明。

#### 性质 $2$

**交换矩阵的两行，行列式的值变为相反数。**也直接用代数方法证明。

#### 性质 $3.1$

矩阵中的一行乘系数 $t$，等价于整个行列式乘系数 $t$，即：
$$
t\cdot
\left|\begin{array}{ccc}
a_1 & a_2 & a_3\\
b_1 & b_2 & b_3\\
c_1 & c_2 & c_3\\
\end{array}\right|
=
\left|\begin{array}{ccc}
t\cdot a_1 & t\cdot a_2 & t\cdot a_3\\
b_1 & b_2 & b_3\\
c_1 & c_2 & c_3\\
\end{array}\right|
$$

#### 性质 $3.2$

对于每一行而言，行列式是线性的，即：
$$
\left|\begin{array}{ccc}
a_1 & a_2 & a_3\\
b_1 & b_2 & b_3\\
c_1 & c_2 & c_3\\
\end{array}\right|
+
\left|\begin{array}{ccc}
d_1 & d_2 & d_3\\
b_1 & b_2 & b_3\\
c_1 & c_2 & c_3\\
\end{array}\right|
=
\left|\begin{array}{ccc}
a_1+d_1 & a_2+d_2 & a_3+d_3\\
b_1 & b_2 & b_3\\
c_1 & c_2 & c_3\\
\end{array}\right|
$$
注意 $|A+B|\neq|A|+|B|$.

#### 性质 $4$

若存在两行相等，则行列式为 $0$；**若存在两行成倍数关系，则行列式为 $0$.**

证明：交换这两行，行列式值变号，又因为行列式值不变，所以 $\operatorname{det}=0$.

#### 性质 $5$

**消元不影响行列式的值。**消元：第 $i$ 行减去第 $j$ 行的若干倍。
$$
\begin{array}{cl}
&\left|\begin{array}{ccc}
a_1 & a_2 & a_3\\
b_1 & b_2 & b_3\\
c_1 - k\cdot a_1& c_2 - k\cdot a_2& c_3-k\cdot a_3\\
\end{array}\right|\\
\underset{\text{Theory 3.2}}{\longrightarrow}&
\left|\begin{array}{ccc}
a_1 & a_2 & a_3\\
b_1 & b_2 & b_3\\
c_1\cdot a_1& c_2& c_3\\
\end{array}\right|
+
\left|\begin{array}{ccc}
a_1 & a_2 & a_3\\
b_1 & b_2 & b_3\\
-k\cdot a_1& - k\cdot a_2 &-k\cdot a_3\\
\end{array}\right|\\
\underset{\text{Theory 4}}{\longrightarrow}&
\left|\begin{array}{ccc}
a_1 & a_2 & a_3\\
b_1 & b_2 & b_3\\
c_1\cdot a_1& c_2& c_3\\
\end{array}\right|
+
0
\end{array}
$$

#### 性质 $6$

若行列式有一整行为 $0$，则行列式为 $0$。

是性质 $3$ 中的 $t=0$ 的特殊情况。

#### 性质 $7$

**倒三角矩阵行列式的值，等于对角线元素的乘积，所以只与对角线上的数有关。**

$$
\left|\begin{array}{ccc}
x & x & x\\
  & x & x\\
  &   & x\\
\end{array}\right|
\underset{\text{Eliminate}}{\longrightarrow}
\left|\begin{array}{ccc}
x &   & \\
  & x & \\
  &   & x\\
\end{array}\right|
$$

形如上面的矩阵，通过消元转化为更简形式即可。

## 求解

根据性质 $5$ 和性质 $7$，直接高斯消元即可。注意每次交换行时，需要$\operatorname{Ans}\leftarrow -\operatorname{Ans}$.

```cpp
int Gauss() {
	int det = 1;
	for(int col = 1; col <= n; col++) {
		for(int row = col + 1; row <= n; row++) {
			while(a[row][col]) {
				int t = a[col][col] / a[row][col];
				for(int i = 1; i <= n; i++) {
					a[col][i] = (a[col][i] + a[row][i] * (p - t)) % p;
                //  a[col][i] = (a[col][i] - a[row][i] * t      ) % p;
				}
				for(int i = 1; i <= n; i++) swap(a[col][i], a[row][i]);
				det *= -1;
			}
		}
	}
	for(int i = 1; i <= n; i++) det = det * a[i][i] % p;
    return (det + p) % p;
}
```

# LGV引理

## LGV引理

参考 [OI Wiki](http://oi-wiki.com/graph/lgv/)，OIer 不需要证明，能完全理解定理内容就不错了。

### 定义如下

- $G$，一张带有边权的**有向无环图**。
- $v_e$， 边 $e$ 的**边权**。
- $\omega(P)$，对于一个依次经过 $u_1,u_2\cdots, u_k$ 的有向路径 $P(u_1\rightsquigarrow u_k)$，$\omega(P)$ 表示其所有**边权之积**；
- $e(u, v)=\sum\limits_{P: u\rightsquigarrow v}\omega(P)$，表示 $u$ 到 $v$ 的**每一条**路径 $P$ 的 $\omega(P)$ **之和**。
- $A=\{a_1, a_2,\cdots a_n\}$，**起点集合**；$B=\{b_1, b_2,\cdots b_n\}$，终点集合。
- $S=\{S_1,S_2,\cdots,S_n\}$，是路径的集合，代表 $n$ 条 $A\rightarrow B$ 的**不相交路径**。形式化地，可以用一个排列 $\sigma(S)$ 来表示，使得 $S_i$ 代表路径 $A_i\rightsquigarrow B_{\sigma(S)_i}$，这组路径满足：$\forall i\neq j$，$S_i$ 和 $S_j$ 无公共点。
- $N(\sigma(S))$，表示排列 $\sigma(S)$ 的逆序对个数。

### 引理

对于矩阵：
$$
M = 
\begin{bmatrix}e(A_1,B_1)&e(A_1,B_2)&\cdots&e(A_1,B_n)\\ e(A_2,B_1)&e(A_2,B_2)&\cdots&e(A_2,B_n)\\
\vdots&\vdots&\ddots&\vdots\\
e(A_n,B_1)&e(A_n,B_2)&\cdots&e(A_n,B_n)
\end{bmatrix}
$$
其行列式满足：
$$
\det(M)=\sum\limits_{S:A\rightarrow B}(-1)^{N(\sigma(S))}\prod\limits_{i=1}^n \omega(S_i)
$$
其中 $S:A\rightarrow B$ 代表：枚举每一组路径（满足上文要求的）。

证明略，下面两道题会说到 LGV 引理的应用方法，

## 模板题

### 题意

有一个 $n\times n$ 的棋盘，左下角为 $(1,1)$，右上角为 $(n,n)$，若一个棋子在点 $(x,y)$，那么走一步只能走到 $(x+1,y)$ 或 $(x,y+1)$。

现在有 $m$ 个棋子，第 $i$ 个棋子一开始放在 $(a_i,1)$，最终要走到 $(b_i,n)$。问有多少种方案，使得每个棋子都能从起点走到终点，且对于所有棋子，走过路径上的点互不相交。输出方案数 $\bmod\ 998244353$ 的值。

两种方案不同当且仅当存在至少一个棋子所经过的点不同。

### 分析

方格图，每次只能向下或向右，不妨设 $a_i$ 和 $b_i$ 均为单调上升的。

因为引理要求**路径不相交**，所以**唯一**的 $\sigma(S)=(1, 2,\cdots, n)$，$N(\sigma(S))=0$，也就是 $a_i$ 和 $b_i$ 配对。

设每条边的边权均为 $1$，则 $\forall \omega(e)=1$，所以：
$$
\det(M)=\sum\limits_{S:A\rightarrow B}(-1)^{N(\sigma(S))}\prod\limits_{i=1}^n \omega(S_i)=\sum\limits_{S:A\rightarrow B}(-1)^0=\sum\limits_{S:A\rightarrow B}1
$$
**右式即为题目所求。**

构造矩阵，$e(a_i,b_j)=\binom{b_j-a_i+n-1}{n-1}$，高斯消元求其行列式即可。

## [NOI2021] 路径交点

[题面](https://www.luogu.com.cn/problem/P7736) 太长就不放了。

同样地，设每条边的边权均为 $1$，则 $\forall \omega(e)=1$，所以：
$$
\det(M)=\sum\limits_{S:A\rightarrow B}(-1)^{N(\sigma(S))}\prod\limits_{i=1}^n \omega(S_i)=\sum\limits_{S:A\rightarrow B}(-1)^{N(\sigma(S))}
$$
引理：**两条路径交点个数的奇偶性，与将其起点和终点直接相连的交点个数的奇偶性相同。**

因此，若我们用 LGV 引理的 $\sigma(S)$ 表示一组路径，若 $N(\sigma(S))\bmod 2=0$，则有路径偶数个交点，反之亦然。

本题求：偶数个交点的方案数量 $-$ 奇数个交点的方案数量，这恰好对应式子中的 $(-1)^{N(\sigma(S))}$.

因此，直接构造矩阵 $M$ 并求行列式，$e(i, j)$ 表示从第一层 $i$ 点到最后一层 $j$ 点的路径数量。

求路径数量可以DP，或者记忆化搜索，不是这道题的难点。

**参考资料：**[**NOI 2021 D1T2 题解 - ix35_ 的博客 - 洛谷博客**](https://www.luogu.com.cn/blog/ix-35/solution-p7736)

# Matrix-Tree 定理

##   基尔霍夫矩阵

基尔霍夫矩阵，又称拉普拉斯矩阵，用于描述一张图的某些属性。

### 无向图

给定一张简单无向图 $G$，其包含 $n$ 个顶点 $v_1, v_2\cdots, v_n$，其拉普拉斯矩阵 $L$ 为：
$$
L_{i,j}=
{\begin{cases}\deg(v_{i})&{\mbox{if}}\ i=j\\-1&{\mbox{if}}\ i\neq j\ {\mbox{and}}\ v_{i}{\mbox{ is adjacent to }}v_{j}\\0&{\mbox{otherwise}}\end{cases}}
$$
也就是说，若设 $D$ 为图的度数矩阵，$A$ 为图的邻接矩阵，则等价地有：
$$
L=D-A
$$


下面是一个对于无向图的例子：

<img src="https://cdn.tonyyin.top/2022/03/05/29850b7bcb979.svg" style="zoom: 67%;" />

|                           度数矩阵                           |                           邻接矩阵                           |                         基尔霍夫矩阵                         |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| ${\textstyle \left({\begin{array}{rrrrrr}2&0&0&0&0&0\\0&3&0&0&0&0\\0&0&2&0&0&0\\0&0&0&3&0&0\\0&0&0&0&3&0\\0&0&0&0&0&1\\\end{array}}\right)}$ | ${\textstyle \left({\begin{array}{rrrrrr}0&1&0&0&1&0\\1&0&1&0&1&0\\0&1&0&1&0&0\\0&0&1&0&1&1\\1&1&0&1&0&0\\0&0&0&1&0&0\\\end{array}}\right)}$ | ${\textstyle \left({\begin{array}{rrrrrr}2&-1&0&0&-1&0\\-1&3&-1&0&-1&0\\0&-1&2&-1&0&0\\0&0&-1&3&-1&-1\\-1&-1&0&-1&3&0\\0&0&0&-1&0&1\\\end{array}}\right)}$ |

### 有向图

有向图存在两个基尔霍夫矩阵，分别为**出度基尔霍夫矩阵** $L^\text{out}$ 和**入度基尔霍夫矩阵** $L^\text{in}$.

其含义与无向图类似，设图 $G$ 的出度矩阵为 $D^\text{out}$，入度矩阵为 $D^\text{in}$，则有：
$$
L^\text{out}=D^\text{out}-A\\
L^\text{in}=D^\text{in}-A
$$

## 定理内容

规定：两个生成树不同，当且仅当存在一条边的被包含情况不同。

**定理仅适用于无重边、无自环的情况。**

### 无向图

设 $t(G)$ 为无向连通图 $G$ 的生成树个数。

设 $L$ 为无向连通图 $G$ 的基尔霍夫矩阵，**任选 $i$，从 $L$ 中删去第 $i$ 行和第 $i$ 列，构造出矩阵 $L^{\prime}$**，则：
$$
t(G)=\det L^{\prime}
$$
如图 $G$ 的基尔霍夫矩阵为 $L=\left[{\begin{array}{rrrr}2&-1&-1&0\\-1&3&-1&-1\\-1&-1&3&-1\\0&-1&-1&2\end{array}}\right]$，则 $L^{\prime}$ 可以为 $L^{\prime}=\left[{\begin{array}{rrrr}2&-1&-1&0\\-1&3&-1&-1\\-1&-1&3&-1\\0&-1&-1&2\end{array}}\right]$.



### 有向图

当根固定时，有向图的生成树分为**根向生成树**和**叶向生成树**，顾名思义，**根向生成树的每条边都指向根**，**叶向生成树的每条边都指向根**。

设 $t^\text{root}(G,s)$ 为以 $s$ 为根节点的根向生成树个数， $t^\text{leaf}(G,s)$ 为以 $s$ 为根节点的叶向生成树个数。

设 $L^\text{out}$ 和 $L^\text{in}$ 为有向连通图 $G$ 的基尔霍夫矩阵，**从中删去第 $k$ 行第 $k$ 列后的矩阵为 ${L^\text{out}}^{\prime}(k)$ 和 ${L^\text{in}}^{\prime}(k)$**.

钦定 $k$ 为所统计生成树的根节点，则有：
$$
t^\text{root}(G,s)=\det{L^\text{out}}^{\prime}(s)\\
$$

$$
t^\text{leaf}(G,s)=\det{L^\text{in}}^{\prime}(s)\\
$$

所以，**有向图的根向树形图个数为 $\sum_{u} t^\text{root}(G,u)$，叶向同理**。

## 简要证明

参见 [Kirchhoff's theorem - Proof outline - Wikipedia](https://en.wikipedia.org/wiki/Kirchhoff%27s_theorem#Proof_outline).

## 参考资料

[Laplacian matrix - Wikipedia](https://en.wikipedia.org/wiki/Laplacian_matrix) 和 [Kirchhoff's theorem - Wikipedia](https://en.wikipedia.org/wiki/Kirchhoff%27s_theorem).

还有例题：[[HEOI2015] 小Z的房间](https://www.luogu.com.cn/problem/P4111).

# 一些资料

- **题单链接：**[【2021赛季专题分享】高斯消元 - TonyYin - 洛谷 - 计算机科学教育新生态](https://www.luogu.com.cn/training/114026)
- **PPT课件：**[课件 - PDF](https://cdn.tonyyin.top/uploads/Gauss.pdf)
- **参考资料：**
- - [高斯消元 - OI Wiki](http://oi-wiki.com/math/gauss/)
  - 李煜东. 算法竞赛进阶指南. 中原出版传媒集团·河南电子音像出版社, 2018.
  - 页面注释部分提到的参考资料。
- **课程资源：**
- - 3blue1brown：[【官方双语_合集】线性代数的本质 - 系列合集_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1ys411472E)
  - Gilbert Strang：[线性代数公开课 - MIT 18.06 Linear Algebra - 系列合集 - 哔哩哔哩](https://www.bilibili.com/video/BV1zx411g7gq)
  - MIT课程提供的英文讲义：[mitmath_1806_ 18.06 course at MIT - Github](https://github.com/mitmath/1806/blob/master/summaries.md)
  - MIT课程的中文笔记：[MIT-Linear-Algebra-Notes - Github](https://github.com/ML-NLPChina/MIT-Linear-Algebra-Notes)，作者：丁坤博、覃立波
- **联系方式  ：**
- - [TonyYin@88.com](mailto:TonyYin@88.com)
