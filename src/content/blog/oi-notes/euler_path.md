---
title: 欧拉图
publishDate: 2021-10-21
description: '欧拉路径、欧拉回路的定义、判定与寻找。'
tags:
  - oi_note

language: '中文'
---

# 定义

**欧拉路径 / 欧拉路**：一条路径，满足：通过所有边恰好一次，且每个点都被走到过。

**欧拉回路**：一条路径，满足：通过所有边恰好一次，且每个点都被走到过，终点和起点相同。

**欧拉图：**存在欧拉回路的图。

**半欧拉图：**存在欧拉路径的图。

# 判定

## 有向图欧拉回路

一个有向图是欧拉图，当且仅当：

1. 图是**强连通**的。
2. 每个点的**出度等于入度**。

## 有向图欧拉路径

一个有向图是半欧拉图，当且仅当：

1. 图是**弱联通**的。（把有向边改成无向边后，图是连通的）
2. 恰好存在一个点 $S$，其**出度比入度多一**；恰好存在一个点 $T$，其**入度比出度多一**。
3. 除 $S$ 和 $T$ 外，**其他点的出度等于入度**。

## 无向图欧拉回路

一个无向图是欧拉图，当且仅当：

1. 图是**连通**的。
2. 每个点**度数均为偶数**。

## 无向图欧拉路径

一个无向图是半欧拉图，当且仅当：

1. 图是**连通**的。
2. 恰有 $0$ 个或 $2$ 个度数为奇数的顶点。（$0$ 个时有欧拉回路，否则 $2$ 个奇度点为起点和终点）

# 寻找

利用 **Hierholzer** 算法。

## Hierholzer 算法

首先，用上面的判定方法，判断是否存在欧拉路径或欧拉回路，并确定起点。

之后，从起点 $S$ 开始进行 `DFS`，每次走过一条边就把边删掉。

过程中，如果一个点的边被删完了，就将其加入答案栈。

最终，从栈顶按顺序输出答案栈中的元素即可。

## 伪代码

$$
\begin{array}{ll}
1 &  \textbf{Input. } \text{The edges of the graph } e , \text{ where each element in } e \text{ is } (u, v) \\
2 &  \textbf{Output. } \text{The vertex of the Euler Road of the input graph}.\\
3 &  \textbf{Method. } \\
4 &  \textbf{Function } \text{Hierholzer } (u) \\
5 &  \qquad \textbf{for } \text{each Edge} e \text{ Begin with } u\\
6 &  \qquad\qquad \textbf{delete } e \text{ from E}\\
7 &  \qquad\qquad \text{Hierholzer }(v) \\
8 &  \qquad\textbf{Insert } v  \text{ into stack}\\
9 &  \textbf{Endfunction}\\
10& \textbf{Print } \text{stack in Reverse order}. 
\end{array}
$$

# P7771 【模板】欧拉路径

给定有向图，保证其弱联通，求字典序最小的欧拉路径。

对每个点的出边按字典序排序，跑 $\textbf{Hierholzer}$ 即可。

代码用优先队列实现了排序这一过程。

```cpp
const int MAXN = 1e5 + 10, MAXM = 2e5 + 10;
int n, m;
priority_queue<int, vector<int>, greater<int> > e[MAXN];
int oud[MAXN], ind[MAXN];
vector<int> path;
void Hierholzer(int x) {
	while(!e[x].empty()) {
		int v = e[x].top(); e[x].pop();
		Hierholzer(v);
	}
	path.push_back(x);
}
int main() {
	n = read(); m = read();
	for(int i = 1; i <= m; i++) {
		int u = read(), v = read();
		e[u].push(v);
		ind[v]++; oud[u]++;
	}
	int ok = 1, S = 0, T = 0;
	for(int i = 1; i <= n; i++) {
		if(oud[i] - ind[i] == 1) {
			if(S) ok = 0;
			else S = i;
		} else if(ind[i] - oud[i] == 1) {
			if(T) ok = 0;
			else T = i;
		} else if(ind[i] != oud[i]) ok = 0;
	}
	if(!ok) printf("No\n");
	else if((S == 0 && T != 0) || (S != 0 && T == 0)) printf("NO\n");
	else {
		if(S == 0) S = 1; //此时S==0&&T==0，有欧拉回路，随便定起点
		Hierholzer(S);
		for(int i = path.size() - 1; i >= 0; i--) printf("%d ", path[i]);
		putchar('\n');
	}
	return 0;
}
```