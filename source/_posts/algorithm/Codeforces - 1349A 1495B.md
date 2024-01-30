---
categories: 
- 算法 
- 题解
date: 2021/4/29 18:30:00
title: Codeforces - 1349A 1495B
katex: true
---
## 1349A - Orac and LCM - [rating 1600]

```
For the multiset of positive integers 𝑠={𝑠1,𝑠2,…,𝑠𝑘}, define the Greatest Common Divisor (GCD) and Least Common Multiple (LCM) of 𝑠 as follow:

gcd(𝑠) is the maximum positive integer 𝑥, such that all integers in 𝑠 are divisible on 𝑥.
lcm(𝑠) is the minimum positive integer 𝑥, that divisible on all integers from 𝑠.
For example, gcd({8,12})=4,gcd({12,18,6})=6 and lcm({4,6})=12. Note that for any positive integer 𝑥, gcd({𝑥})=lcm({𝑥})=𝑥.

Orac has a sequence 𝑎 with length 𝑛. He come up with the multiset 𝑡={lcm({𝑎𝑖,𝑎𝑗}) | 𝑖<𝑗}, and asked you to find the value of gcd(𝑡) for him. In other words, you need to calculate the GCD of LCMs of all pairs of elements in the given sequence.

Input
The first line contains one integer 𝑛 (2≤𝑛≤100000).

The second line contains 𝑛 integers, 𝑎1,𝑎2,…,𝑎𝑛 (1≤𝑎𝑖≤200000).

Output
Print one integer: gcd({lcm({𝑎𝑖,𝑎𝑗}) | 𝑖<𝑗}).
```

#### 题意

给定 $n$ 个整数，求出 $\gcd(\{\operatorname{lcm}({a_i,a_j})_{ 1\le i<j\le n} \})$

#### 解法

假设最后结果为 $ans$，则对于 $ans$ 的每个质数因子 $m=p^k$ ( $p$ 为质数，$k>=1$ )，在原数组中必有至少 $n-1$ 个整数包含该因子，即只有 $0$ 个或者 $1$ 个整数不包含该因子。

反证：如果有 $2$ 个整数不包含该因子，则他们的 $lcm$ 也必定不包含该因子，所以最终结果不可能包含该因子。

对每个质数 $p$，找出每个整数对应的最大 $k$ 值，因为最多可以有一个整数不包含 $p^k$，在所有值中第二小的 $k$ 则为 $ans$ 中可以包含的最大 $k$ 值。

https://codeforces.com/contest/1349/submission/121674760

## 1495B - Let's Go Hiking - [rating 1900]


```
On a weekend, Qingshan suggests that she and her friend Daniel go hiking. Unfortunately, they are busy high school students, so they can only go hiking on scratch paper.

A permutation 𝑝 is written from left to right on the paper. First Qingshan chooses an integer index 𝑥 (1≤𝑥≤𝑛) and tells it to Daniel. After that, Daniel chooses another integer index 𝑦 (1≤𝑦≤𝑛, 𝑦≠𝑥).

The game progresses turn by turn and as usual, Qingshan moves first. The rules follow:

If it is Qingshan's turn, Qingshan must change 𝑥 to such an index 𝑥′ that 1≤𝑥′≤𝑛, |𝑥′−𝑥|=1, 𝑥′≠𝑦, and 𝑝𝑥′<𝑝𝑥 at the same time.
If it is Daniel's turn, Daniel must change 𝑦 to such an index 𝑦′ that 1≤𝑦′≤𝑛, |𝑦′−𝑦|=1, 𝑦′≠𝑥, and 𝑝𝑦′>𝑝𝑦 at the same time.
The person who can't make her or his move loses, and the other wins. You, as Qingshan's fan, are asked to calculate the number of possible 𝑥 to make Qingshan win in the case both players play optimally.

Input
The first line contains a single integer 𝑛 (2≤𝑛≤10^5) — the length of the permutation.

The second line contains 𝑛 distinct integers 𝑝1,𝑝2,…,𝑝𝑛 (1≤𝑝𝑖≤𝑛) — the permutation.

Output
Print the number of possible values of 𝑥 that Qingshan can choose to make her win.
```

#### 题意

输入一个所有元素均不相等的正整数数组后，$Alice$ 和 $Bob$ 各选定一个位置，$Alice$ 每次只能往比当前数字小的相邻数字移动，$Bob$ 每次只能往比当前数字大的相邻数字移动，游戏过程中 $Alice$ 和 $Bob$ 不能相会于同一个点，$Alice$ 先手，最先不能移动者为负。

如果现在由 $Alice$ 先选择位置，在所有 $n$ 个位置中，有几个位置是必胜位置？

必胜位置：不管 $Bob$ 选择其他任何位置，$Alice$ 都能获胜

要求时间复杂度最大为 $O(n logn)$

#### 解法

动态规划的复杂度在 $O(n^2)$，达不到题目要求。

考虑单调性，设最长单调序列长度为 $l$ ，并且在数组中共有 $x$ 个长度为 $l$ 的单调序列，然后分情况讨论；
- 如果 $x=1$
  - 如果 $Alice$ 选择的位置在最长序列之外，$Bob$ 只需选择最长序列的最小点， $Alice$ 必输。
  - 如果 $Alice$ 选择的位置在最长序列之中（非最高点），$Bob$ 只需选择 $Alice$ 下方的一点， $Alice$ 必输。
  - 如果 $Alice$ 选择的位置在最长序列之最高点，并且 $l$ 是偶数， $Bob$ 只需选择最长序列的最小点，$Alice$ 无论从最长序列往下还是不从最长序列往下都必输。
  - 如果 $Alice$ 选择的位置在最长序列之最高点，并且 $l$ 是奇数， $Bob$ 只需选择最长序列的最小点上面的一点，$Alice$ 无论从最长序列往下还是不从最长序列往下都必输。


- 如果 $x\ge3$
即使 $Alice$ 选择了两条最长序列的交界点，$Bob$ 还是能选择剩余一条最长序列的最小点，$Alice$ 必输。

- 剩下讨论 $x=2$
  - 如果两条最长序列不相交，情况就和 $x\ge3$ 相同了，$Alice$ 必输。
  - 如果两条最长序列相交，并且相交点为最低点，由于 $Alice$ 选择不了最低点，情况和 $x=1$ 类似，$Alice$ 必输。
  - 如果两条最长序列相交，并且相交点为最高点，图形为山峰形， $Alice$ 可以选择最高点，此时 $Bob$ 如果想在山腰卡住 $Alice$ ，$Alice$ 就可以走另一边了。所以 $Bob$ 只能选择左或者右的最小点，在这种情况下，如果 $l$ 是偶数， $Alice$ 还是输了，但如果 $l$ 是奇数，$Alice$ 就能主动和 $Bob$ 会面，此最高点为 $Alice$ 的必胜位置。

代码实现中先找出唯一的山峰是否存在，再判断 $l$ 奇偶性。

https://codeforces.com/contest/1495/submission/114518829