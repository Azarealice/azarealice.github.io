---
categories: 技术 - 算法
tags: 技术
date: 2021/1/30 15:40:00
---

### LeetCode - 进阶 - 状态压缩DP

#### [1659. 最大化网格幸福感](https://leetcode-cn.com/problems/maximize-grid-happiness/)

给你四个整数 m、n、introvertsCount 和 extrovertsCount 。有一个 m x n 网格，和两种类型的人：内向的人和外向的人。总共有 introvertsCount 个内向的人和 extrovertsCount 个外向的人。

请你决定网格中应当居住多少人，并为每个人分配一个网格单元。 注意，不必 让所有人都生活在网格中。

每个人的 幸福感 计算如下：

内向的人 开始 时有 120 个幸福感，但每存在一个邻居（内向的或外向的）他都会 失去  30 个幸福感。
外向的人 开始 时有 40 个幸福感，每存在一个邻居（内向的或外向的）他都会 得到  20 个幸福感。
邻居是指居住在一个人所在单元的上、下、左、右四个直接相邻的单元中的其他人。

网格幸福感 是每个人幸福感的 总和 。 返回 最大可能的网格幸福感 。

示例 1：

![img](https://i.loli.net/2021/01/30/jp6Ix48iC2ng35m.png)

```
输入：m = 2, n = 3, introvertsCount = 1, extrovertsCount = 2
输出：240
解释：假设网格坐标 (row, column) 从 1 开始编号。
将内向的人放置在单元 (1,1) ，将外向的人放置在单元 (1,3) 和 (2,3) 。
- 位于 (1,1) 的内向的人的幸福感：120（初始幸福感）- (0 * 30)（0 位邻居）= 120
- 位于 (1,3) 的外向的人的幸福感：40（初始幸福感）+ (1 * 20)（1 位邻居）= 60
- 位于 (2,3) 的外向的人的幸福感：40（初始幸福感）+ (1 * 20)（1 位邻居）= 60
网格幸福感为：120 + 60 + 60 = 240
上图展示该示例对应网格中每个人的幸福感。内向的人在浅绿色单元中，而外向的人在浅紫色单元中。
```
提示：
```
1 <= m, n <= 5
0 <= introvertsCount, extrovertsCount <= min(m * n, 6)
```

<!--more-->

解法：从上至下，从左至右进行记忆化dfs，要点为，当dfs到点(x,y)时，需要知道的信息有哪些？其实需要知道的仅仅是(x-1,y)和(x,y-1)两个点的摆放情况，所以维护一个长度为列长的三进制数即可，其实第一位为(x-1,y)的状态，末位为(x,y-1)的状态，进入下一步时，抛弃第一位，剩余位左移一位，并将当前点状态压入末位。

```java
class Solution {

    private int statemax = 1, mod = 0, R = 0, C = 0;
    private int[][][][][] dp;

    public int getMaxGridHappiness(int m, int n, int introvertsCount, int extrovertsCount) {

        R = m;
        C = n;
        for (int i = 0; i < n; i++) {
            statemax *= 3;
        }
        mod = statemax / 3;

        dp = new int[m][n][introvertsCount + 1][extrovertsCount + 1][statemax];
        return dfs(0, 0, introvertsCount, extrovertsCount, 0);
    }

    private int dfs(int x, int y, int in, int ex, int last) {

        if (x == R) {
            return 0;
        }
        if (y == C) {
            return dfs(x + 1, 0, in, ex, last); // 有到达边界，直接看下一行
        }
        if (dp[x][y][in][ex][last] != 0) {
            return dp[x][y][in][ex][last];
        }

        // 不安排
        int res = dfs(x, y + 1, in, ex, last % mod * 3);

        // 安排内向
        if (in != 0) {
            int t1 = 120, up = last / mod, left = last % 3;
            if (x - 1 >= 0 && up != 0) {
                t1 -= 30;
                t1 += up == 1 ? -30 : 20;
            }
            if (y - 1 >= 0 && left != 0) {
                t1 -= 30;
                t1 += left == 1 ? -30 : 20;
            }
            res = Math.max(res, t1 + dfs(x, y + 1, in - 1, ex, last % mod * 3 + 1));
        }

        // 安排外向
        if (ex != 0) {
            int t2 = 40, up = last / mod, left = last % 3;
            if (x - 1 >= 0 && up != 0) {
                t2 += 20;
                t2 += up == 1 ? -30 : 20;
            }
            if (y - 1 >= 0 && left != 0) {
                t2 += 20;
                t2 += left == 1 ? -30 : 20;
            }
            res = Math.max(res, t2 + dfs(x, y + 1, in, ex - 1, last % mod * 3 + 2));
        }
        return dp[x][y][in][ex][last] = res;
    }
}

```

#### [1349. 参加考试的最大学生数](https://leetcode-cn.com/problems/maximum-students-taking-exam/)

给你一个 m * n 的矩阵 seats 表示教室中的座位分布。如果座位是坏的（不可用），就用 '#' 表示；否则，用 '.' 表示。

学生可以看到左侧、右侧、左上、右上这四个方向上紧邻他的学生的答卷，但是看不到直接坐在他前面或者后面的学生的答卷。请你计算并返回该考场可以容纳的一起参加考试且无法作弊的最大学生人数。

学生必须坐在状况良好的座位上。

示例 1：

```
输入：seats = [["#",".","#","#",".","#"],
              [".","#","#","#","#","."],
              ["#",".","#","#",".","#"]]
输出：4
解释：教师可以让 4 个学生坐在可用的座位上，这样他们就无法在考试中作弊。 
```


示例 2：

```
输入：seats = [[".","#"],
              ["#","#"],
              ["#","."],
              ["#","#"],
              [".","#"]]
输出：3
解释：让所有学生坐在可用的座位上。
```


示例 3：

```
输入：seats = [["#",".",".",".","#"],
              [".","#",".","#","."],
              [".",".","#",".","."],
              [".","#",".","#","."],
              ["#",".",".",".","#"]]
输出：10
解释：让学生坐在第 1、3 和 5 列的可用座位上。
```

解法：练习用，方法参考上一题。

```java
class Solution {

    int[][][] dp;
    int row;
    int col;
    int mod;
    char[][] seats;

    public int maxStudents(char[][] seats) {
        this.seats = seats;
        row = seats.length;
        col = seats[0].length;

        int maxState = 1;
        for (int i = 0; i < col; i++) {
            maxState *= 2;
        }

        mod = maxState;
        maxState *= 2;

        dp = new int[row][col][maxState];

        return dfs(0, 0, 0);
    }

    private int dfs(int r, int c, int lastState) {
        if (r == row) {
            return 0;
        }
        if (c == col) {
            return dfs(r + 1, 0, lastState);
        }
        if (seats[r][c] == '#') {
            dp[r][c][lastState] = dfs(r, c + 1, lastState % mod * 2);
            return dp[r][c][lastState];
        }
        if (dp[r][c][lastState] > 0) {
            return dp[r][c][lastState];
        }
        int left = c == 0 ? 0 : lastState % 2;
        int upperLeft = c == 0 ? 0 : lastState / mod;
        int upperRight = c == col - 1 ? 0 : lastState / (mod / 4) % 2;

        int res = 0;
        if (left == 0 && upperLeft == 0 && upperRight == 0) {
            res = dfs(r, c + 1, lastState % mod * 2 + 1) + 1;
        }
        res = Math.max(res, dfs(r, c + 1, lastState % mod * 2));

        dp[r][c][lastState] = res;
        return res;
    }
}
```

