---
title: 【ZROI-20普转提-Day1-T4】魔法师
publishDate: 2021-01-12
description: '算法题解'
tags:
  - oi_solution

language: '中文'
---

【ZROI-20普转提-Day1-T4】魔法师

# $\rm{Description}$

有 $m$ 次询问，每次询问给定一次操作。操作分为两种：向集合 $A$ 中添加一个物品或从集合 $A$ 中删除一个物品。对于每个物品，有两个属性：

- 物品的类别 $t_i$ 
- 物品的威力 $p_i$

现在要从集合 $A$ 中选取一个子集 $B$，使得 $B$ 中物品的威力和最大，且 $B$ 满足： 

- 对于每个类别 $t_i$，最多选取 $t_i$ 本书；
- 一共最多能够选取 $n$ 本书（题目给定 $n$）。

在每次询问之后，输出最大威力和。

# $\rm{Solution}$

## $\rm{Subtask}$ $\rm{1}$

容易想到使用 `multiset` 进行暴力，期望得分 $30$ 分。

## $\rm{Subtask}$ $\rm{2}$

考虑如何处理“**最多取 $t_i$ 个，最多取 $n$ 个**”。

对于每种物品 $t$，开两个 `multiset`，分别包含威力在前 $t$ 大的元素和其他元素。

显然对于每个物品 $i$，只有在自己的种类中，威力排在前 $t_i$，才有可能在集合 $B$ 中。

于是再开两个全局的 `multiset`，第一个是集合 $B$，第二个是所有前 $t_i$ 大的元素中，不在 $B$ 中的元素。

此时对于一个添加操作来说，先判断是不是前 $t_i$ 大；若是，则看能不能添加到 $B$ 集合中；若能，则更新答案。

对于一个删除操作来说，同理。先判断是不是前 $t_i$ 大，若是，则把小的集合中最大的元素放到前 $t_i$ 大中；若同时还在集合 $B$ 中，则处理方式类似，只不过还要更新答案。

# $\rm{Code}$

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;
inline int read() {
	int ret = 0; char ch = getchar();
	while(ch < '0' || ch > '9') ch = getchar();
	while(ch <= '9' && ch >= '0') {
		ret = ret * 10 + ch - '0';
		ch = getchar();
	}
	return ret;
}
const int MAXN = 3e5 + 10;
int n, m;
multiset<int> a_big[MAXN], a_small[MAXN];
multiset<int> big, small;
//用 a_big[i] 存目前有的第 i 种书中，前i大的数值，升序
//用 a_small[i] 存目前有的第 i 种书中，其他的数值，升序
//用 big 存最终取出的最多 n 本书，一定从 a_big 中取出，升序
//用 small 存其他的数值
int maxt, ans;
void out(int x) {
	if(x >= 10) out(x / 10);
	putchar(x % 10 + '0');
}
void add(int x) {//将数值 x 添加到 big/small 中
	if(big.size() < n) {//如果big中还能加，就直接加
		big.insert(x);
		ans += x;
	} else {
		//否则先找到big中的最小值mmin
		int mmin = *big.begin();
		if(x > mmin) {//如果x能顶替big中的一个元素，那么顶替
			ans -= mmin; ans += x;
			small.insert(mmin);
			big.erase(big.begin());
			big.insert(x);
		} else {//否则添加到small
			small.insert(x);
		}
	}
}
void del(int x) {//将数值 x 从 big/small 中删除
	if(big.find(x) != big.end()) { //如果在big里面能找到
		ans -= x;
		big.erase(big.find(x));
		if(!small.empty()) {
			//找到small里面最大的，放到big里，之后把x从big中删除
			auto it = small.end(); it--;
			int mmax = *it;
			small.erase(it);
			big.insert(mmax);
			ans += mmax;
		}
	} else { //不在big中，就一定在small中，直接删
		small.erase(small.find(x));
	}
}
signed main() {
	n = read(); m = read();
	for(int i = 1; i <= m; i++) {
		
		int op = 0;
		char ch = getchar();
		while(ch < 'A' || ch > 'Z') ch = getchar();
		if(ch == 'B') op = 1;
		else if(ch == 'R') op = 2;
		int t = read(), p = read(); maxt = max(maxt, t);
		
		if(op == 1) { //Borrow
			//要把元素p加入进去
			if(a_big[t].size() < t) {//如果有空余就加
				a_big[t].insert(p);
				add(p);
			} else {
				//否则找到big里的最小值，和p进行比较
				int mmin = *a_big[t].begin();
				if(p > mmin) {//能把big中的一个元素顶替下来
					del(mmin);
					a_small[t].insert(mmin);
					a_big[t].erase(a_big[t].begin());
					add(p);
					a_big[t].insert(p);
				} else {//不能，添加到small
					a_small[t].insert(p);
				}
			}
		} else { //Return
			if(a_big[t].find(p) != a_big[t].end()){
				//如果在big里面，就找到small中的最大值，放到big里面，然后删掉p
				a_big[t].erase(a_big[t].find(p));
				del(p);
				if(!a_small[t].empty()) {
					auto it = a_small[t].end(); it--;
					int mmax = *it;
					a_small[t].erase(it);
					a_big[t].insert(mmax);
					add(mmax);
				}
			} else {//在small里面直接删
				a_small[t].erase(a_small[t].find(p));
			}
		}
		out(ans); putchar('\n');
	}
	return 0;
}
```

