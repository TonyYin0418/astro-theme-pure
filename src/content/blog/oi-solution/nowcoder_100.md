---
title: 【牛客练习赛100】小红的构造题
publishDate: 2022-06-19
description: '算法题解'
tags:
  - oi_solution

language: '中文'
---

## 题意

https://ac.nowcoder.com/acm/contest/11251/D

小红想让你构造一个长度不超过 $200000$ 的字符串，其中包含 $k$ 个 $\texttt {red}$ 子序列。你能帮帮她吗？ 

子序列的定义：在原串中必须按顺序，可以不连续。例如，$\texttt {reddd}$ 有3个：$\underline{\texttt{red}}\texttt{dd}$，$\underline{\texttt{re}}\texttt{de}\underline{\texttt{d}}$，$\underline{\texttt{r}}\texttt{ed}\underline{\texttt{ed}}$.

若无法构造，输出 $-1$，多解输出任意。

数据范围：$0\leq k\leq 10^{14}$.

## 题解

考虑字符串：
$$
\texttt{rerererererere}\dots
$$
在第一个 $\texttt{re}$ 后面加 $x$ 个 $\texttt{d}$，可稳定增加 $x$ 个子序列；

在第二个 $\texttt{re}$ 后面加 $x$ 个 $\texttt{d}$，可稳定增加 $3x$ 个子序列；

以此类推，

在第 $k$ 个 $\texttt{re}$ 后面加 $x$ 个 $\texttt{d}$，可稳定增加 $\frac{k\cdot (k+1)}{2}x$ 个子序列。

于是把字符串长度缩小到 $n^{\frac{1}{3}}$ 级别，可以证明这样的长度一定小于 $200000$.

```cpp
#include<bits/stdc++.h>
using namespace std;
int tong[101010];
int main(){
    long long n,i,j,k,p;
    cin>>n;
    if(n==0)return cout<<"d",0;
    for(i=1;i*i*i/6<=n;i++);
    i--;
    for(j=i;j>0;j--){
        tong[j]+=n/(j*(j+1)/2);
        n%=j*(j+1)/2;
    }
    for(j=1;j<=i;j++){
        cout<<"re";
        while(tong[j]--)cout<<"d";
    }
}
```

