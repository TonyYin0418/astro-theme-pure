---
title: 【ZROI-19普转提-Day3-T4】DAG
publishDate: 2020-12-21
description: '算法题解'
tags:
  - oi_solution

language: '中文'
---

题目地址：[【普转提七联测 Day 3】DAG - 题目 - Zhengrui Online Judge](http://zhengruioi.com/problem/1007)

## 题意

给出一个无向图，请你给边定向成为一个 $\rm{DAG}$，使得最长路最短。求这个最短长度。

$1\leq n\leq 8$，$1\leq m\leq 20$.

## 解法

### $\rm{subtask}$ $\rm{1}$

暴力？

### $\rm{subtask}$ $\rm{2}$

发现**结论**：要求的答案就是无向图中的最小染色数减一。

考虑如何**证明**：

将无向图的点使用尽可能少的颜色染色，使得相邻的点颜色不同。显然任意两个颜色之间都有边直接相连，否则可以将这两个颜色合并成一个。

于是我们只考虑颜色之间的关系。将一个颜色看作一个点，那么无向图转化为一个由颜色作为顶点的完全图。现在给这个完全图的边定向，使其成为 $\rm{DAG}$，显然其中的最长路长度为 $n-1$.

按照颜色图中，各个颜色之间的关系，在原图中对应的连边，这样每个点被经过一次且无环，最长路长度为 $n-1$.

此时问题转化为：求无向图的最小染色数，也就是求无向图最少由多少个独立集组成。

观察数据范围，一眼看出状压$\rm{DP}$.

将每个点在集合中是存在进行二进制压缩，设置状态 $\operatorname{DP}_{i}$ 表示 $i$ 这个集合，最少由多少个独立集组成。

枚举 $i$ 的所有子独立集进行转移即可。

时间复杂度 $\mathcal{O}(3^n)$.

## 代码

```cpp
#include <bits/stdc++.h>
#define MAXN 20
#define MAXM 200
using namespace std;
int n, m;
int u[MAXM], v[MAXM];
bool is_ind_set[(1 << MAXN)];
int dp[(1 << MAXN)];
int main() {
	scanf("%d%d", &n, &m);
	for(int i = 1; i <= m; i++) {
		scanf("%d%d", &u[i], &v[i]);
	}
	for(int i = 0; i < (1 << n); i++) {
		is_ind_set[i] = true;
		for(int j = 1; j <= m; j++) {
			if((i & (1 << (u[j] - 1))) && (i & (1 << (v[j] - 1)))) {
				is_ind_set[i] = false;
				break;
			}
		}
	}
	memset(dp, 0x3f, sizeof(dp));
	dp[0] = 0;
	for(int i = 0; i < (1 << n); i++) {
		for(int j = i; j; j = (j - 1) & i) {
			if(is_ind_set[j]) {
				dp[i] = min(dp[i ^ j] + 1, dp[i]);
			}
		}
	}
	printf("%d\n", dp[(1 << n) - 1] - 1);
	return 0;
}
```

