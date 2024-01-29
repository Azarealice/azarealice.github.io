---
categories: 
- 算法 
- Codeforces
date: 2021/4/29 18:30:00
title: Codeforces - 1349A 1495B
---
#### 1349A - Orac and LCM - [rating 1600]

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

题意

给定n个整数，两两一组配对共n*(n-1)/2对求出各自的LCM，再求出所有LCM值的GCD

解法

假设最后结果为ans，则对于ans的每个质数因子m=p^k(p为质数，k>=1)，在原数组中必有至少n-1个整数包含该因子，即只有0个或者1个整数不包含该因子。

反证过程：如果有2个整数不包含该因子，则他们的LCM也必定不包含该因子，所以最终结果不可能包含该因子。

对每个质数p，找出每个整数对应的最大k值，因为最多可以有一个整数不包含p^k，在所有值中第二小的k则为ans中可以包含的最大k值。

https://codeforces.com/contest/1349/submission/121674760

#### 1495B - Let's Go Hiking - [rating 1900]


```
On a weekend, Qingshan suggests that she and her friend Daniel go hiking. Unfortunately, they are busy high school students, so they can only go hiking on scratch paper.

A permutation 𝑝 is written from left to right on the paper. First Qingshan chooses an integer index 𝑥 (1≤𝑥≤𝑛) and tells it to Daniel. After that, Daniel chooses another integer index 𝑦 (1≤𝑦≤𝑛, 𝑦≠𝑥).

The game progresses turn by turn and as usual, Qingshan moves first. The rules follow:

If it is Qingshan's turn, Qingshan must change 𝑥 to such an index 𝑥′ that 1≤𝑥′≤𝑛, |𝑥′−𝑥|=1, 𝑥′≠𝑦, and 𝑝𝑥′<𝑝𝑥 at the same time.
If it is Daniel's turn, Daniel must change 𝑦 to such an index 𝑦′ that 1≤𝑦′≤𝑛, |𝑦′−𝑦|=1, 𝑦′≠𝑥, and 𝑝𝑦′>𝑝𝑦 at the same time.
The person who can't make her or his move loses, and the other wins. You, as Qingshan's fan, are asked to calculate the number of possible 𝑥 to make Qingshan win in the case both players play optimally.

Input
The first line contains a single integer 𝑛 (2≤𝑛≤105) — the length of the permutation.

The second line contains 𝑛 distinct integers 𝑝1,𝑝2,…,𝑝𝑛 (1≤𝑝𝑖≤𝑛) — the permutation.

Output
Print the number of possible values of 𝑥 that Qingshan can choose to make her win.
```

题意

输入一个非负数组后（数组中所有相邻元素均不相等），A和B各选定一个位置，A每次只能往比当前数字小的相邻数字移动，B每次只能往比当前数字大的相邻数字移动，游戏过程中AB不能相会于同一个点，A先手，最先不能移动者为负。
如果现在由A先选择位置，在所有n个位置中，有几个位置是必胜位置？（不管B选择其他任何位置，A都能获胜），要求时间复杂度O(n)

解法

动态规划的复杂度在O(n^2)，达不到题目要求。

这里考虑单调性，设最长单调序列长度为l，并且在数组中共有x个长度为l的单调序列，以下分情况讨论；
如果x=1；
如果A选择的位置在最长序列之外，B只需选择最长序列的最小点，A必输。
如果A选择的位置在最长序列之中（非最高点），B只需选择A下方的一点，A必输。
如果A选择的位置在最长序列之最高点，并且l是偶数，B只需选择最长序列的最小点，A无论从最长序列往下还是不从最长序列往下都必输。
如果A选择的位置在最长序列之最高点，并且l是奇数，B只需选择最长序列的最小点上面的一点，A无论从最长序列往下还是不从最长序列往下都必输。

如果x=3；
即使A选择了两条最长序列的交界点，B还是能选择剩余一条最长序列的最小点，A必输。

剩下讨论x=2；
如果两条最长序列不相交，情况就和x=3一样了，A必输。
如果两条最长序列相交，并且相交点为最低点，由于A选择不了最低点，情况和x=1类似，A必输。
如果两条最长序列相交，并且相交点为最高点，图形为山峰形，A可以选择最高点，此时B如果想在山腰卡住A，A就可以走另一边了，B的最优解变为选择左或者右的最小点，如果l是偶数，A还是输了，如果l是奇数，此最高点为A的必胜位置。

代码实现中先找出唯一的山峰是否存在，再判断l奇偶性。

https://codeforces.com/contest/1495/submission/114518829