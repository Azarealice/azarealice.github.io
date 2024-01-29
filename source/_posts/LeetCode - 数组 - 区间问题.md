---
categories: 技术 - 算法
tags: 技术
date: 2020/7/12 16:15:00
---

### LeetCode - 数组 - 区间问题

#### [45. 跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)


给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

**示例:**

```
输入: [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**说明:**

假设你总是可以到达数组的最后一个位置。

<!--more-->

解法：标记一个end和maxPosition，分别代表这一步的最右边界，和下一步的最右边界（在这一步的所有可能位置中，寻找最大右边界，即为下一步的最右边界）。

```java
public int jump(int[] nums) {
    int length = nums.length;
    int end = 0;
    int maxPosition = 0;
    int steps = 0;
    for (int i = 0; i < length - 1; i++) {
      maxPosition = Math.max(maxPosition, i + nums[i]);
      if (i == end) {
        end = maxPosition;
        steps++;
      }
    }
    return steps;
  }
```

#### [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

示例 1:

```
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
```


示例 2:

```
输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
```

解法：类似于45题，如果maxPosition大于最右位置即为true，如果下一步最后和这一步最后相等，即代表不能再往右。

```java
public boolean canJump(int[] nums) {

    int end = 0;
    int max = 0;

    for (int i = 0; i < nums.length; i++) {
        max = Math.max(max, i + nums[i]);
        if (max >= nums.length - 1) {
            return true;
        }
        if (i == end) {
            if (max == end) {
                break;
            }
            end = max;
        }
    }

    return false;
}
```

#### [1024. 视频拼接](https://leetcode-cn.com/problems/video-stitching/)

你将会获得一系列视频片段，这些片段来自于一项持续时长为 T 秒的体育赛事。这些片段可能有所重叠，也可能长度不一。

视频片段 clips\[i\] 都用区间进行表示：开始于 clips\[i\]\[0\] 并于 clips\[i\]\[1\] 结束。我们甚至可以对这些片段自由地再剪辑，例如片段 [0, 7] 可以剪切成 [0, 1] + [1, 3] + [3, 7] 三部分。

我们需要将这些片段进行再剪辑，并将剪辑后的内容拼接成覆盖整个运动过程的片段（[0, T]）。返回所需片段的最小数目，如果无法完成该任务，则返回 -1 。

示例 1：

```
输入：clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]], T = 10
输出：3
解释：
我们选中 [0,2], [8,10], [1,9] 这三个片段。
然后，按下面的方案重制比赛片段：
将 [1,9] 再剪辑为 [1,2] + [2,8] + [8,9] 。
现在我们手上有 [0,2] + [2,8] + [8,10]，而这些涵盖了整场比赛 [0, 10]。
```


示例 2：

```
输入：clips = [[0,1],[1,2]], T = 5
输出：-1
解释：
我们无法只用 [0,1] 和 [0,2] 覆盖 [0,5] 的整个过程。
```


示例 3：

```
输入：clips = [[0,1],[6,8],[0,2],[5,6],[0,4],[0,3],[6,7],[1,3],[4,7],[1,4],[2,5],[2,6],[3,4],[4,5],[5,7],[6,9]], T = 9
输出：3
解释： 
我们选取片段 [0,4], [4,7] 和 [6,9] 。
```


示例 4：

```
输入：clips = [[0,4],[2,8]], T = 5
输出：2
解释：
注意，你可能录制超过比赛结束时间的视频。
```


提示：

```
1 <= clips.length <= 100
0 <= clips[i][0] <= clips[i][1] <= 100
0 <= T <= 100
```

解法：贪心，维护一个数组表示【覆盖该点的最右边界】，然后向右推进即可。

```java
 public int videoStitching(int[][] clips, int T) {

    int[] right = new int[T];
    for (int[] clip : clips) {
        for (int i = clip[0]; i < clip[1] && i < T; i++) {
            right[i] = Math.max(right[i], clip[1]);
        }
    }

    int ret = 0;
    int current = 0;

    while (current < T) {
        if (right[current] <= current) {
            return -1;
        }
        current = right[current];
        ret++;
    }

    return ret;
}
```

#### [1326. 灌溉花园的最少水龙头数目](https://leetcode-cn.com/problems/minimum-number-of-taps-to-open-to-water-a-garden/)

在 x 轴上有一个一维的花园。花园长度为 n，从点 0 开始，到点 n 结束。

花园里总共有 n + 1 个水龙头，分别位于 [0, 1, ..., n] 。

给你一个整数 n 和一个长度为 n + 1 的整数数组 ranges ，其中 ranges[i] （下标从 0 开始）表示：如果打开点 i 处的水龙头，可以灌溉的区域为 [i -  ranges[i], i + ranges[i]] 。

请你返回可以灌溉整个花园的 最少水龙头数目 。如果花园始终存在无法灌溉到的地方，请你返回 -1 。

 

示例 1：

![img](https://i.loli.net/2020/07/12/sSzrDnLejpgB3dI.png)

```
输入：n = 5, ranges = [3,4,1,1,0,0]
输出：1
解释：
点 0 处的水龙头可以灌溉区间 [-3,3]
点 1 处的水龙头可以灌溉区间 [-3,5]
点 2 处的水龙头可以灌溉区间 [1,3]
点 3 处的水龙头可以灌溉区间 [2,4]
点 4 处的水龙头可以灌溉区间 [4,4]
点 5 处的水龙头可以灌溉区间 [5,5]
只需要打开点 1 处的水龙头即可灌溉整个花园 [0,5] 。
示例 2：

输入：n = 3, ranges = [0,0,0,0]
输出：-1
解释：即使打开所有水龙头，你也无法灌溉整个花园。
示例 3：

输入：n = 7, ranges = [1,2,1,0,2,1,0,1]
输出：3
示例 4：

输入：n = 8, ranges = [4,0,0,0,0,0,0,0,4]
输出：2
示例 5：

输入：n = 8, ranges = [4,0,0,0,4,0,0,0,4]
输出：1
```


提示：

```
1 <= n <= 10^4
ranges.length == n + 1
0 <= ranges[i] <= 100
```

解法：类似于1024题

```java
public int minTaps(int n, int[] ranges) {

    int[] right = new int[n + 1];

    for (int i = 0; i <= n; i++) {
        int l = Math.max(i - ranges[i], 0);
        int r = Math.min(i + ranges[i], n);
        for (int j = l; j <= r; j++) {
            right[j] = Math.max(right[j], r);
        }
    }

    int ret = 0;
    int current = 0;

    while (current < n) {
        if (right[current] <= current) {
            return -1;
        }
        current = right[current];
        ret++;
    }

    return ret;
}
```