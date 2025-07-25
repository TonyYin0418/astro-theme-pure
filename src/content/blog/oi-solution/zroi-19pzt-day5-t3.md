---
title: 【ZROI-19普转提-Day5-T3】背包
publishDate: 2020-12-25
description: '算法题解'
tags:
  - oi_solution

language: '中文'
---

## 题意

有 $n$ 个物品，编号为 $1$ ~ $n$，每个物品有重量 $w_i$，价值 $v_i$.

有 $m$ 个背包，编号为 $1$ ~ $m$，每个背包有容量上限 $t_i$.

物品 $i$ 能够放入背包 $j$，当且仅当 $t_j \geq w_i$。

现在选出 $n$ 个物品，使它们能够通过交换顺序满足：物品的重量和价值从左到右均单调不降。且这 $n$ 个物品能分别放入不同的背包中。求 $\rm{Max\_n}$.

## 解法

### $\rm{Part}$ $\rm{1}$

这 $n$ 个物品需要满足两个条件：

1.  物品的重量和价值从左到右均单调不降。
2.  都能放进背包中。

容易想到将 $n$ 个物品按照重量升序排序，将 $m$ 个背包按照容量升序排序。

### $\rm{Part}$ $\rm{2}$

要满足性质2，可以贪心地将 $w_n$ 与 $t_m$ 对应，$w_{n-1}$ 与 $t_{m-1}$ 对应……

如果这样对应之后还是无法满足性质2，那其他的对应方法一定也无法满足。

### $\rm{Part}$ $\rm{3}$

现在要满足性质1.

目前物品的重量已经单调不降了，只需要让价值单调不降。由于与背包对应的策略是从后往前的，

考虑对物品从后往前DP。

设置状态，$\operatorname{DP}_{i}$ 表示最左端的物品价值为 $i$ 时，最多选多少个物品。

假设当前我们枚举到了第 $i$ 个物品。如果要选 $i$，那么已经选好的物品只有可能在 $i$ 右侧，且右侧点的价值一定都大于等于 $v_i$.

记：

$$
suf=\max_{k\geq v_i}(\operatorname{DP}_{k})

$$

如果我们要选 $i$，那么 $i$ 一定放入第 $m-suf$ 号背包。能放入的条件为 $w_i\leq t_{m-suf}$.

（也就是说，如果要选 $i$，那么一定把 $i$ 接在右侧能选的最长的数列上面，否则对后面的答案没有贡献）

求 $suf$ 的过程可以使用树状数组进行优化。（把所有 $\operatorname{DP}_{i}$ 放到树状数组中）

```cpp
for(int i = n; i >= 1; i--){
    int suf = query(a[i].v); //求max(dp[k]), k >= a[i].v
    if(a[i].w <= t[m - suf]){ //能满足条件2
        ans = max(ans, suf + 1); //upd. ans
        add(a[i].v, suf + 1); //add(v, len);
    }
}
```

## 代码

```cpp
#include <bits/stdc++.h>
#define MAXN 100008
using namespace std;

int T, n, m, t[MAXN];
struct NODE{
    int w, v;
    friend bool operator < (const NODE &A, const NODE &B){
        return A.w < B.w || (A.w == B.w && A.v < B.v);
    }
} a[MAXN];
int c[MAXN];
int p[MAXN], cnt = 0;
void init() {
    sort(t + 1, t + m + 1);
    
    for(int i = 1; i <= n; i++) p[i] = a[i].v;
    cnt = n;
    sort(p + 1, p + cnt + 1);
    cnt = unique(p + 1, p + cnt + 1) - p - 1;
    for(int i = 1; i <= n; i++) a[i].v = lower_bound(p + 1, p + cnt + 1, a[i].v) - p;
    
    sort(a + 1, a + n + 1);
    memset(c, 0, sizeof(c));
}
int lowbit(int x) {
    return x & (-x);
}
void add(int x, int val) {
    for(int i = x; i; i -= lowbit(i)) {
        c[i] = max(c[i], val);
    }
}
int query(int v) {
    int ret = 0;
    for(int i = v; i <= n; i += lowbit(i)) {
        ret = max(ret, c[i]);
    }
    return ret;
}
int main() {
    scanf("%d", &T);
    while(T--) {
        scanf("%d", &n);
        for(int i = 1; i <= n; i++) {
            scanf("%d%d", &a[i].w, &a[i].v);
        }
        scanf("%d", &m);
        for(int i = 1; i <= m; i++) {
            scanf("%d", &t[i]);
        }
        init();
        int ans = 0;
        for(int i = n; i >= 1; i--) {
            int suf = query(a[i].v);
            if(a[i].w <= t[m - suf]) {
                ans = max(ans, suf + 1);
                add(a[i].v, suf + 1);
            }
        }
        cout << ans << endl;
    }
    return 0;
}

```