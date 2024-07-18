---
categories: 
- 算法 
- 题解
date: 2024-07-18 11:58:30
title: Codeforces - 1995G
katex: true
---
## 1995G - Ultra-Meow - [rating 2000]

```
K1o0n gave you an array 𝑎 of length 𝑛, consisting of numbers 1,2,…,𝑛. Accept it? Of course! But what to do with it? Of course, calculate MEOW(𝑎).

Let MEX(𝑆,𝑘) be the 𝑘-th **positive** (strictly greater than zero) integer in ascending order that is not present in the set 𝑆. Denote MEOW(𝑎) as the sum of MEX(𝑏,|𝑏|+1), over all **distinct** subsets 𝑏 of the array 𝑎.

Examples of MEX(𝑆,𝑘) values for sets:

- MEX({3,2},1)=1, because 1 is the first positive integer not present in the set;
- MEX({4,2,1},2)=5, because the first two positive integers not present in the set are 3 and 5;
- MEX({},4)=4, because there are no numbers in the empty set, so the first 4 positive integers not present in it are 1,2,3,4.

Input

The first line contains a single integer 𝑡𝑡 (1≤𝑡≤10^4) — the number of test cases.

In a single line of each test case, an integer 𝑛 (1≤𝑛≤5000) is entered, the size of the array of gifted numbers.

It is guaranteed that the sum of 𝑛^2 over all test cases does not exceed 25⋅10^6.

Output

For each test case, output a single number — MEOW(𝑎). Since it may be very large, output it modulo 10^9+7.
```

#### 题意

函数 $MEX(S,k)$ 代表升序正整数列中第 $k$ 个不属于集合 $S$ 的数。

例：$MEX([1,2,4],2)=5$​，$MEX([2,3],1)=1$​，$MEX([],1)=1$​​

现在给你一个正整数列 $S=1,2,\dots,n$ ，求出 $\sum_{\substack{s\in S}} MEX(s,\vert s\vert+1)$， 其中 $\vert s\vert$ 为 $s$ 的大小。

#### 解法

记 $S$ 的长度为 $n$ ， $s$ 的长度为 $l$ ，前 $l+1$  个不属于集合 $s$ 的升序正整数列为 $t$ ， $m = MEX(s,\vert s\vert+1) = MEX(s,l+1) = \max(x)_{x\in t}$ ，以下分情况讨论：

1. 对于 $2*l+1 \le n  $ 的子序列，其 $m$ 值不固定，对于给定的 $l$ 中每个可能的值 $m\in [l+1,2*l+1]$  ，$m$ 即为 $t$ 序列的**最大值**， $m$ 左侧有 $m-1$ 个空位，在其中放入**长度为 $l$ 的 $t$ 剩余序列**和**长度为 $m-1-l$ 的 $s$ 序列**，排列个数为 $l \choose m-1$ ，$m$ 右侧有 $n-m$ 个空位，在其中放入**长度为 $l-(m-1-l)$ 的 $s$ 剩余序列**，排列个数为 $l - (m - 1 - l) \choose n - m $，故每个 $m$ 值的排列个数为 ${l \choose m-1} * {l - (m - 1 - l)) \choose n - m }$，这部分的 $m$ 总和为 $\sum_{l=0}^{2*l+1\le n} \sum_{m=l+1}^{m\le 2*l+1} {l \choose m-1} * {l - (m - 1 - l)) \choose n - m }$
2. 对于 $2*l+1 \gt n  $ 的子序列，序列 $s$ 和序列 $t$ 的总长度超过 $n$ ， 则其 $m$ 值固定为 $2*l+1$ ，长度为 $l$ 的子序列的个数为 ${l \choose n}$ ，这部分的 $m$ 总和为 $\sum_{2*l+1\gt n}^{l\le n} (2*l+1) * {l \choose n}$

故 $\sum_{\substack{s\in S}} MEX(s,\vert s\vert+1) = \sum_{l=0}^{2*l+1\le n} \sum_{m=l+1}^{m\le 2*l+1} {l \choose m-1} * {l - (m - 1 - l \choose n - m } + \sum_{2*l+1\gt n}^{l\le n} (2*l+1) * {l \choose n}$​

https://codeforces.com/contest/1992/submission/271131606
