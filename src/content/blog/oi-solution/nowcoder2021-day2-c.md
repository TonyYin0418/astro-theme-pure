---
title: 【2021牛客OI赛前集训营】树数树
publishDate: 2021-10-07
description: '算法题解'
tags:
  - oi_solution

language: '中文'
---


题目来源：[2021牛客OI赛前集训营-提高组（第二场）](https://ac.nowcoder.com/acm/contest/20107/C)

## 题意

给定一棵 $n$ 个点的**有根树**，根为 $1$。

长度为 $m$ 的序列 $a$ 满足：

- $\forall i\in(1, m]$，$a_i$ 为 $a_{i-1}$ 的祖先或 $a_{i-1}$ 是 $a_{i}$ 的**祖先**；
- $\forall 1\leq i < j\leq m$，$a_i\neq a_j$.

求**最长的**序列 $a$ 的**长度**。

## 分析

### $\rm{40\sim 50pts}$

大样例提示很明显。

对任意的二叉树（包括链），答案均为节点个数；对菊花图，答案为 $3$。

### $\rm{100pts}$

上面的部分对正解提示也很明显。

容易发现，选序列的策略是：对于一个点 $u$，先选一个它的子树处理，再选其自己，再选另一个子树处理。

要让答案最大化，所以我们要**选出两个答案最大的子树**。

因此，我们用堆维护：对于子树中所有**未被选取的点**，将其（及其子树的一部分）选上之后，可以对增加的长度。

每次处理完之后，向上合并堆。（用可并堆或`multiset`）

![](https://cdn.tonyyin.top/2021/10/07/0870c1a220ff3.png)

上图序列 $a$ 的选取方法如下：

![](https://cdn.tonyyin.top/2021/10/07/31b0a44a0c97f.png)



被同一个节点选取的两个子树，其根节点颜色相同。无填充色即不选取。

## 代码

```cpp
#include <bits/stdc++.h>
#include<ext/pb_ds/priority_queue.hpp>
using namespace std;
using namespace __gnu_pbds;
const int MAXN = 100005;
int T, n;
vector<int> G[MAXN];
__gnu_pbds::priority_queue<int,less<int>,pairing_heap_tag>q[MAXN];
void dfs(int u,int father) {
    for(int v:G[u]) {
        if(v == father) continue;
        dfs(v, u);
        q[u].join(q[v]);
        q[v].clear();
    }
    int x = 1;
    for(int i = 1; i <= 2; i++)
    {
        if(q[u].empty()) break;
        x += q[u].top(); q[u].pop();
    }
    q[u].push(x);
    return;
}
void solve() {
   	scanf("%d", &n);
    for(int i = 1; i < n; i++) {
        int x, y;
        scanf("%d%d", &x, &y);
        G[x].push_back(y);
        G[y].push_back(x);
    }
    dfs(1,0);
    printf("%d\n", q[1].top());
    for(int i = 1; i <= n; i++) {
        G[i].clear();
		q[i].clear();
	}
    return;
}
int main() {
    scanf("%d", &T);
    while(T--) solve();
    return 0;
}
```

