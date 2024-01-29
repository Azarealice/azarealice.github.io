---
categories: 技术 - 算法
tags: 技术
date: 2020/7/12 22:46:00
---

### LeetCode - 堆排序

#### [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

示例 1:

```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```


示例 2:

```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```


说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

<!--more-->

```java
public int findKthLargest(int[] nums, int k) {
    int heapSize = nums.length;
    buildMaxHeap(nums, heapSize);
    for (int i = nums.length - 1; i >= nums.length - k + 1; --i) {
        swap(nums, 0, i);
        --heapSize;
        maxHeapify(nums, 0, heapSize);
    }
    return nums[0];
}

public void buildMaxHeap(int[] a, int heapSize) {
    for (int i = heapSize / 2; i >= 0; --i) {
        maxHeapify(a, i, heapSize);
    }
}

public void maxHeapify(int[] a, int i, int heapSize) {
    int l = i * 2 + 1, r = i * 2 + 2, largest = i;
    if (l < heapSize && a[l] > a[largest]) {
        largest = l;
    }
    if (r < heapSize && a[r] > a[largest]) {
        largest = r;
    }
    if (largest != i) {
        swap(a, i, largest);
        maxHeapify(a, largest, heapSize);
    }
}

public void swap(int[] a, int i, int j) {
    int temp = a[i];
    a[i] = a[j];
    a[j] = temp;
}
```

#### [218. 天际线问题](https://leetcode-cn.com/problems/the-skyline-problem/)

城市的天际线是从远处观看该城市中所有建筑物形成的轮廓的外部轮廓。给你所有建筑物的位置和高度，请返回由这些建筑物形成的 天际线 。

每个建筑物的几何信息由数组 buildings 表示，其中三元组 buildings[i] = [lefti, righti, heighti] 表示：

lefti 是第 i 座建筑物左边缘的 x 坐标。
righti 是第 i 座建筑物右边缘的 x 坐标。
heighti 是第 i 座建筑物的高度。
天际线 应该表示为由 “关键点” 组成的列表，格式 [[x1,y1],[x2,y2],...] ，并按 x 坐标 进行 排序 。关键点是水平线段的左端点。列表中最后一个点是最右侧建筑物的终点，y 坐标始终为 0 ，仅用于标记天际线的终点。此外，任何两个相邻建筑物之间的地面都应被视为天际线轮廓的一部分。

注意：输出天际线中不得有连续的相同高度的水平线。例如 [...[2 3], [4 5], [7 5], [11 5], [12 7]...] 是不正确的答案；三条高度为 5 的线应该在最终输出中合并为一个：[...[2 3], [4 5], [12 7], ...]

 

示例 1：

![img](https://i.loli.net/2021/01/02/kCWsJiVd7u1P4fN.jpg)

```
输入：buildings = [[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]
输出：[[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]
```

解释：
图 A 显示输入的所有建筑物的位置和高度，
图 B 显示由这些建筑物形成的天际线。图 B 中的红点表示输出列表中的关键点。
示例 2：

```
输入：buildings = [[0,2,3],[2,5,3]]
输出：[[0,3],[5,0]]
```


提示：

```
1 <= buildings.length <= 104
0 <= lefti < righti <= 231 - 1
1 <= heighti <= 231 - 1
buildings 按 lefti 非递减排序
```

解法：用优先队列存储当前最大高度，另外存储所有左右端点，依次遍历推进。

```java
public List<List<Integer>> getSkyline(int[][] buildings) {

    PriorityQueue<int[]> priorityQueue = new PriorityQueue<>((o1, o2) -> o2[2] - o1[2]);
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> bIndexs = new ArrayList<>(buildings.length * 2);
    for (int[] building : buildings) {
        bIndexs.add(building[0]);
        bIndexs.add(building[1]);
    }
    Collections.sort(bIndexs);

    int bIndex = 0;
    int curHeight = 0;
    int prev = -1;
    for (int index : bIndexs) {
        if (index == prev) {
            continue;
        }
        while (priorityQueue.peek() != null && priorityQueue.peek()[1] <= index) {
            priorityQueue.poll();
        }

        while (bIndex < buildings.length && buildings[bIndex][0] == index) {
            priorityQueue.offer(buildings[bIndex]);
            bIndex++;
        }

        int newHeight = priorityQueue.isEmpty() ? 0 : priorityQueue.peek()[2];
        if (newHeight != curHeight) {
            List<Integer> skyLine = new ArrayList<>();
            skyLine.add(index);
            skyLine.add(newHeight);
            result.add(skyLine);

            curHeight = newHeight;
        }
        prev = index;
    }
    return result;
}
```

#### [407. 接雨水 II](https://leetcode-cn.com/problems/trapping-rain-water-ii/)

给你一个 m x n 的矩阵，其中的值均为非负整数，代表二维高度图每个单元的高度，请计算图中形状最多能接多少体积的雨水。

示例：

```
给出如下 3x6 的高度图:
[
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]

返回 4 。
```

下雨后，雨水将会被存储在这些方块中。总的接雨水量是4。

提示：

```
1 <= m, n <= 110
0 <= heightMap[i][j] <= 20000
```

解法：从边缘向内广度优先遍历

```java
public int trapRainWater(int[][] heights) {
    if (heights == null || heights.length == 0) {
        return 0;
    }
    int n = heights.length;
    int m = heights[0].length;
    // 用一个vis数组来标记这个位置有没有被访问过
    boolean[][] vis = new boolean[n][m];
    // 优先队列中存放三元组 [x,y,h] 坐标和高度
    PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> o1[2] - o2[2]);

    // 先把最外一圈放进去
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (i == 0 || i == n - 1 || j == 0 || j == m - 1) {
                pq.offer(new int[]{i, j, heights[i][j]});
                vis[i][j] = true;
            }
        }
    }
    int res = 0;
    // 方向数组，把dx和dy压缩成一维来做
    int[] dirs = {-1, 0, 1, 0, -1};
    while (!pq.isEmpty()) {
        int[] poll = pq.poll();
        // 看一下周围四个方向，没访问过的话能不能往里灌水
        for (int k = 0; k < 4; k++) {
            int nx = poll[0] + dirs[k];
            int ny = poll[1] + dirs[k + 1];
            // 如果位置合法且没访问过
            if (nx >= 0 && nx < n && ny >= 0 && ny < m && !vis[nx][ny]) {
                // 如果外围这一圈中最小的比当前这个还高，那就说明能往里面灌水啊
                if (poll[2] > heights[nx][ny]) {
                    res += poll[2] - heights[nx][ny];
                }
                // 如果灌水高度得是你灌水后的高度了，如果没灌水也要取高的
                pq.offer(new int[]{nx, ny, Math.max(heights[nx][ny], poll[2])});
                vis[nx][ny] = true;
            }
        }
    }
    return res;
}
```

#### [1383. 最大的团队表现值](https://leetcode-cn.com/problems/maximum-performance-of-a-team/)

公司有编号为 1 到 n 的 n 个工程师，给你两个数组 speed 和 efficiency ，其中 speed[i] 和 efficiency[i] 分别代表第 i 位工程师的速度和效率。请你返回由最多 k 个工程师组成的 最大团队表现值 ，由于答案可能很大，请你返回结果对 10^9 + 7 取余后的结果。

团队表现值 的定义为：一个团队中「所有工程师速度的和」乘以他们「效率值中的最小值」。

 

示例 1：

```
输入：n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 2
输出：60
解释：
我们选择工程师 2（speed=10 且 efficiency=4）和工程师 5（speed=5 且 efficiency=7）。他们的团队表现值为 performance = (10 + 5) * min(4, 7) = 60 。
```


示例 2：

```
输入：n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 3
输出：68
解释：
此示例与第一个示例相同，除了 k = 3 。我们可以选择工程师 1 ，工程师 2 和工程师 5 得到最大的团队表现值。表现值为 performance = (2 + 10 + 5) * min(5, 4, 7) = 68 。
```


示例 3：

```
输入：n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 4
输出：72
```


提示：

```
1 <= n <= 10^5
speed.length == n
efficiency.length == n
1 <= speed[i] <= 10^5
1 <= efficiency[i] <= 10^8
1 <= k <= n
```

解法：将工程师效率**从高到低**排列，依次将工程师加入池子，故加到第i个工程师的时候，可能最高总效率就为（前i个工程师中前k个速度最高的总和 * 第i个工程师的效率），将速度依次加入最小堆中，每次加入新工程师的时候，将新工程师的速度替换掉一个最小速度（前提是速度池已满，此处必须替换，如果不替换的话速度效率同步减少，就没有比前面i-1工程师的总效率高的可能）

```java
public int maxPerformance(int n, int[] speed, int[] efficiency, int k) {
    int[][] items = new int[n][2];
    for (int i = 0; i < n; i++) {
        items[i][0] = speed[i];
        items[i][1] = efficiency[i];
    }
    Arrays.sort(items, (a, b) -> b[1] - a[1]);
    PriorityQueue<Integer> queue = new PriorityQueue<>();
    long res = 0, sum = 0;
    for (int i = 0; i < n; i++) {
        if (queue.size() > k - 1) {
            sum -= queue.poll();
        }
        res = Math.max(res, (sum + items[i][0]) * items[i][1]);
        queue.add(items[i][0]);
        sum += items[i][0];
    }
    return (int) (res % ((int) 1e9 + 7));
}
```