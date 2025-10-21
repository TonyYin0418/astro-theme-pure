---
title: 【ZROI-19普及组-Day2-T4】与非门树
publishDate: 2020-12-19
description: '算法题解'
tags:
  - oi_solution

language: '中文'
---

## 题意

给定一棵 $n$ 个节点的有根树，每个点有点权。对于所有叶子节点，点权为 $0$ 或 $1$。非叶子节点分为两类，对于第一类，点权为其所有儿子节点的**点权与非和**；对于第二类，点权恒为 $h_i\in[0， 1]$，题目给定 $h_i$.

现在给叶子节点赋值。

存在 $A$ 种不同的赋值方法使得：**若将所有第二类点变为第一类点，根节点的点权不变**，一共存在 $B$ 种不同的赋值方法。

求 $\frac{A}{B}\bmod 998244353$.

## 解法

~上面的部分只用于理解题意，与Solution部分基本无关。~

考虑树形DP.

定义 $\operatorname{cnt}[x]$ 表示与 $x$ 直接相连的叶子节点个数。

定义 $x$ 的理论值为：如果所有的与非门都能正常工作，$x$ 的点权。

定义 $x$ 的实际值为：在实际情况下， $x$ 的点权。

设置状态 $\operatorname{DP}_{x,0/1,0/1}$ 表示 $x$ 的点权，理论值为 $0/1$，实际值为 $0/1$ 时的赋值方法种类数量。

### $\rm{Part}$ $\rm{1}$

**如果 $x$ 不是一个坏了的与非门**，那么考虑此时四种状态如何从 $x$ 的儿子转移上来。

##### $\operatorname{DP}_{x, 0, 0}$ 的转移

对于 $\operatorname{DP}_{x, 0, 0}$，理论值和实际值都为 $0$，所以对于所有 $x$ 的儿子，理论值和实际值必须都为 $1$，即：

$$
\operatorname{DP}_{x, 0, 0} = \prod_{\text{i是x的儿子}}\operatorname{DP}_{i, 1, 1}

$$

##### $\operatorname{DP}_{x, 0, 1}$ 的转移

对于 $\operatorname{DP}_{x, 0, 1}$，理论值为 $0$，实际值为 $1$，说明 $x$ 的所有儿子，必须满足理论值为 $1$，且至少一个实际值不为 $1$

容易想到容斥原理，易得 $\operatorname{DP}_{x, 0, 1}$ 等于：所有儿子的理论值都为 $1$，实际值随意的情况数，减去所有儿子理论值都为 $1$ ，实际值都为 $1$ 的情况数。

又因为所有儿子理论值都为 $1$ ，实际值都为 $1$ 的情况数等于 $\operatorname{DP}_{x, 0, 0}$，所以有转移方程：

$$
\operatorname{DP}_{x, 0, 1}=\prod_{\text{i是x的儿子}}(\operatorname{DP}_{i, 1, 0} +\operatorname{DP}_{i, 1, 1} )-\operatorname{DP}_{x, 0, 0}

$$

##### $\operatorname{DP}_{x, 1, 0}$ 的转移

与 $\operatorname{DP}_{x, 0, 1}$ 类似，可以得到转移方程：

$$
\operatorname{DP}_{x, 1, 0}=\prod_{\text{i是x的儿子}}(\operatorname{DP}_{i, 0, 1} +\operatorname{DP}_{i, 1, 1} )-\operatorname{DP}_{x, 0, 0}

$$

##### $\operatorname{DP}_{x, 1, 1}$ 的转移

由于 $\operatorname{DP}_{x, 0, 0}$，$\operatorname{DP}_{x, 0, 1}$，$\operatorname{DP}_{x, 1, 0}$，$\operatorname{DP}_{x, 1, 1}$ 四种情况为所有的状态，所以只需要用所有的状态减去前面三个状态，就可以得到 $\operatorname{DP}_{x, 1, 1}$ 了，即：

$$
\operatorname{DP}_{x, 1, 1}=2^{\operatorname{cnt}[x]}\cdot \prod_{\text{i是x的儿子}}(\operatorname{DP}_{i, 0, 0}+\operatorname{DP}_{i, 0, 1}+\operatorname{DP}_{i, 1, 0}+\operatorname{DP}_{i, 1, 1})-(\operatorname{DP}_{x, 0, 0}+\operatorname{DP}_{x, 0, 1}+\operatorname{DP}_{x, 1, 0})

$$

至此，我们已经知道了：若 $x$ 不是一个坏了的与非门，如何从 $x$ 的儿子转移到 $x$ 上。

```cpp
void dfs(int x) {
    dp[x][0][0] = dp[x][0][1] = dp[x][1][0] = 1;
    dp[x][1][1] = power(2, cnt[x]);
    for(int i = head[x]; i; i = edge[i].nxt) {
        int ne = edge[i].to;
        dfs(ne);
        dp[x][0][0] *= dp[ne][1][1];
        dp[x][0][1] *= dp[ne][1][0] + dp[ne][1][1];
        dp[x][1][0] *= dp[ne][0][1] + dp[ne][1][1];
        dp[x][1][1] *= dp[ne][0][0] + dp[ne][0][1] + dp[ne][1][0] + dp[ne][1][1];
    }
    dp[x][0][1] -= dp[x][0][0];
    dp[x][1][0] -= dp[x][0][0];
    dp[x][1][1] -= dp[x][0][0] + dp[x][0][1] + dp[x][1][0];
}
```

### $\rm{Part}$ $\rm{2}$

下面处理特殊情况：$x$ 是坏了的与非门。

**如果 $x$ 只能输出 $0$，不能输出 $1$.**

因为 $\rm{Part}$ $\rm{1}$ 中的实际值，是在默认 $x$ 不是坏了的与非门的前提下考虑的。如果 $x$ 是坏了的与非门，那么 $\operatorname{DP}_{x}$ 就只与 $x$ 儿子的理论值相关了。

所以 $\rm{Part}$ $\rm{1}$ 中的 $\operatorname{DP}_{x, 1, 1}$ 应该归到 $\operatorname{DP}_{x, 1, 0}$ 中，$\operatorname{DP}_{x, 0, 1}$ 应该归到 $\operatorname{DP}_{x, 0, 0}$ 中，即：

```cpp
if(t[now].sta == 0) {
    dp[now][0][0] += dp[now][0][1];
    dp[now][1][0] += dp[now][1][1];
    dp[now][0][1] = dp[now][1][1] = 0;
}
```

**如果 $x$ 只能输出 $1$，不能输出 $0$.**

同理，将 $\rm{Part}$ $\rm{1}$ 中的 $\operatorname{DP}_{x, 0, 0}$ 应该归到 $\operatorname{DP}_{x, 0, 1}$ 中，$\operatorname{DP}_{x, 1, 0}$ 应该归到 $\operatorname{DP}_{x, 1, 1}$ 中.

```cpp
if(t[now].sta == 1) {
    dp[now][0][1] += dp[now][0][0];
    dp[now][1][1] += dp[now][1][0];
    dp[now][0][0] = dp[now][1][0] = 0;
}
```

## 代码

代码没开long long，没取模（因为加上取模之后代码宽度直接爆炸）

```cpp
#include <bits/stdc++.h>
#define MAXN 200008
using namespace std;
int n, root;
struct Point{int fa, cnt_son, sta;} t[MAXN];
struct Edge{int to, nxt;} edge[2 * MAXN];
int head[MAXN], cnt = 0;
int deg[MAXN], res[MAXN];
void add(int x, int y) {
    edge[++cnt].to = y;
    edge[cnt].nxt = head[x];
    head[x] = cnt;
    deg[x]++;
}
int dp[MAXN][2][2];
int power(int x, int k) {
    int ret = 1;
    while(k) {
        if(k & 1) ret = ret * x;
        k >>= 1;
        x = x * x;
    }
    return ret;
}
int inv(int x, int p) {
    return power(x, p - 2) % p;
}
void dfs(int now) {
    res[now] = t[now].cnt_son - deg[now];
    dp[now][0][0] = dp[now][0][1] = dp[now][1][0] = 1;
    dp[now][1][1] = power(2, res[now]);
    for(int i = head[now]; i; i = edge[i].nxt) {
        int ne = edge[i].to;
        dfs(ne);
        res[now] += res[ne];
        dp[now][0][0] *= dp[ne][1][1];
        dp[now][0][1] *= dp[ne][1][0] + dp[ne][1][1];
        dp[now][1][0] *= dp[ne][0][1] + dp[ne][1][1];
        dp[now][1][1] *= dp[ne][0][0] + dp[ne][0][1] + dp[ne][1][0] + dp[ne][1][1];
    }
    dp[now][0][1] -= dp[now][0][0];
    dp[now][1][0] -= dp[now][0][0];
    dp[now][1][1] -= dp[now][0][0] + dp[now][0][1] + dp[now][1][0];
    if(t[now].sta == 0) {
        dp[now][0][0] += dp[now][0][1];
        dp[now][1][0] += dp[now][1][1];
        dp[now][0][1] = dp[now][1][1] = 0;
    }
    if(t[now].sta == 1) {
        dp[now][0][1] += dp[now][0][0];
        dp[now][1][1] += dp[now][1][0];
        dp[now][0][0] = dp[now][1][0] = 0;
    }
}
int main() {
    cin >> n;
    for(int i = 1; i <= n; i++) {
        cin >> t[i].fa >> t[i].cnt_son >> t[i].sta;
        if(t[i].fa == 0) {
            root = i;
            continue;
        }
        add(t[i].fa, i);
    }
    dfs(root);
    int ans = (dp[root][0][0] + dp[root][1][1]) * inv(power(2, res[root]), 998244353);
    cout << ans << endl;
    return 0;
}

```