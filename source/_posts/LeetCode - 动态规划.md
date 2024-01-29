---
categories: 技术 - 算法
tags: 技术
date: 2020/7/25 15:59:00
---

### LeetCode - 动态规划

#### [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```


示例 2：

```
输入: "cbbd"
输出: "bb"
```

<!--more-->

解法：p(i,j)代表代表index从i到j的子字符串是否为回文字符串。

存在更优解法：manacher算法，参考：https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zui-chang-hui-wen-zi-chuan-by-leetcode-solution/

```java
public String longestPalindrome(String s) {

        if (s.length() == 0) {
            return s;
        }
        int len = s.length();
        int max = 1;
        int left = 0;

        boolean[][] p = new boolean[len][len];

        for (int i = 0; i < len; i++) {
            p[i][i] = true;
        }

        for (int j = 1; j < len; j++) {
            for (int i = 0; i < j; i++) {
                if (s.charAt(i) != s.charAt(j)) {
                    p[i][j] = false;
                } else {
                    if (j - i <= 2) {
                        p[i][j] = true;
                    } else {
                        p[i][j] = p[i + 1][j - 1];
                    }
                }

                if (p[i][j] && j - i + 1 > max) {
                    max = j - i + 1;
                    left = i;
                }
            }
        }

        return s.substring(left, left + max);
    }
}
```

#### [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

给定一个整数数组 prices ，它的第 i 个元素 prices[i] 是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 

示例 1：

```
输入：k = 2, prices = [2,4,1]
输出：2
解释：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
```


示例 2：

```
输入：k = 2, prices = [3,2,6,5,0,3]
输出：7
解释：在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
```


提示：

```
0 <= k <= 109
0 <= prices.length <= 1000
0 <= prices[i] <= 1000
```

解法：维护两个数组sell和buy代表第i天第j次交易后的利润


```java
public int maxProfit(int k, int[] prices) {
    int n = prices.length;
    int[][] buy = new int[n + 1][k + 1];
    int[][] sell = new int[n + 1][k + 1];
    for (int i = 0; i < n + 1; i++) {
        Arrays.fill(buy[i], Integer.MIN_VALUE);
    }

    for (int i = 1; i < n + 1; i++) {
        for (int j = 1; j < k + 1; j++) {
            buy[i][j] = Math.max(buy[i - 1][j], sell[i - 1][j - 1] - prices[i - 1]);
            sell[i][j] = Math.max(sell[i - 1][j], buy[i - 1][j] + prices[i - 1]);
        }
    }
    return sell[n][k];
}
```

#### [1235. 规划兼职工作](https://leetcode-cn.com/problems/maximum-profit-in-job-scheduling/)

你打算利用空闲时间来做兼职工作赚些零花钱。

这里有 n 份兼职工作，每份工作预计从 startTime[i] 开始到 endTime[i] 结束，报酬为 profit[i]。

给你一份兼职工作表，包含开始时间 startTime，结束时间 endTime 和预计报酬 profit 三个数组，请你计算并返回可以获得的最大报酬。

注意，时间上出现重叠的 2 份工作不能同时进行。

如果你选择的工作在时间 X 结束，那么你可以立刻进行在时间 X 开始的下一份工作。

示例 1：

![img](https://i.loli.net/2021/01/10/EbA3HSMkZoRFvq8.png)

```
输入：startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70]
输出：120
解释：
我们选出第 1 份和第 4 份工作， 
时间范围是 [1-3]+[3-6]，共获得报酬 120 = 50 + 70。
```


示例 2：

![img](https://i.loli.net/2021/01/10/uM7jx2mUS6ErXQz.png)

```
输入：startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]
输出：150
解释：
我们选择第 1，4，5 份工作。 
共获得报酬 150 = 20 + 70 + 60。
```


示例 3：

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/sample3_1584.png)

```
输入：startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4]
输出：6
```


提示：

```
1 <= startTime.length == endTime.length == profit.length <= 5 * 10^4
1 <= startTime[i] < endTime[i] <= 10^9
1 <= profit[i] <= 10^4
```

解法：动态规划，向前寻找做了i的前提下的最右工作。

```java
class Job {

    int start;
    int end;
    int profit;

    Job(int start, int end, int profit) {
        this.start = start;
        this.end = end;
        this.profit = profit;
    }
}

public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {
    int n = startTime.length;
    int[] dp = new int[n + 1];
    Job[] job = new Job[n];
    for (int i = 0; i < n; i++) {
        job[i] = new Job(startTime[i], endTime[i], profit[i]);
    }
    Arrays.sort(job, Comparator.comparingInt(o -> o.end));
    dp[0] = job[0].profit;
    for (int i = 1; i < n; i++) {
        dp[i] = Math.max(dp[i - 1], job[i].profit);
        for (int j = i - 1; j >= 0; j--) {
            if (job[j].end <= job[i].start) {
                dp[i] = Math.max(dp[i], dp[j] + job[i].profit);
                break;
            }
        }
    }
    return dp[n - 1];
}
```