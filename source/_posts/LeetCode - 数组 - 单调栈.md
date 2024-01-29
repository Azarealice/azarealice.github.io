---
categories: 技术 - 算法
tags: 技术
date: 2020/7/5 21:25:00
---

### LeetCode - 数组 - 单调栈

#### [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

![](https://i.loli.net/2020/07/12/179Nybk65T4nalL.png)

上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。

示例:

```
输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6
```

解法：单调递减栈

<!--more-->

```java
public int trap(int[] height) {
    if (height == null) {
        return 0;
    }
    Stack<Integer> stack = new Stack<>();
    int ans = 0;
    for (int i = 0; i < height.length; i++) {
        while (!stack.isEmpty() && height[stack.peek()] < height[i]) {
            int curIdx = stack.pop();
            // 如果栈顶元素一直相等，那么全都pop出去，只留第一个。
            while (!stack.isEmpty() && height[stack.peek()] == height[curIdx]) {
                stack.pop();
            }
            if (!stack.isEmpty()) {
                int stackTop = stack.peek();
                // stackTop此时指向的是此次接住的雨水的左边界的位置。右边界是当前的柱体，即i。
                // Math.min(height[stackTop], height[i]) 是左右柱子高度的min，减去height[curIdx]就是雨水的高度。
                // i - stackTop - 1 是雨水的宽度。
                ans += (Math.min(height[stackTop], height[i]) - height[curIdx]) * (i - stackTop - 1);
            }
        }
        stack.add(i);
    }
    return ans;
}
```

#### [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 ![](https://i.loli.net/2020/07/12/8oKbulTHjvZyNUi.png)

以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 [2,1,5,6,2,3]。

 ![](https://i.loli.net/2020/07/12/QAT2zMH8RJp3Wln.png)

图中阴影部分为所能勾勒出的最大矩形面积，其面积为 10 个单位。

示例:

```
输入: [2,1,5,6,2,3]
输出: 10
```

解法：

对于每一列，需要找到它左边/右边第一个小于它高度的列，考虑维护一个单调递增栈

```java
public int largestRectangleArea(int[] heights) {
    int n = heights.length;
    int[] left = new int[n];
    int[] right = new int[n];

    Stack<Integer> mono_stack = new Stack<Integer>();
    for (int i = 0; i < n; ++i) {
        while (!mono_stack.isEmpty() && heights[mono_stack.peek()] >= heights[i]) {
            //出栈时，第i列为第peek()列右边的第一个小于其高度的列
            right[mono_stack.peek()] = i;
            mono_stack.pop();
        }
        left[i] = (mono_stack.isEmpty() ? -1 : mono_stack.peek());
        mono_stack.push(i);
    }

    while (!mono_stack.isEmpty()){
        right[mono_stack.peek()] = n;
        mono_stack.pop();
    }

    int ans = 0;
    for (int i = 0; i < n; ++i) {
        ans = Math.max(ans, (right[i] - left[i] - 1) * heights[i]);
    }
    return ans;
}
```


#### [85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)

给定一个仅包含 0 和 1 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。

示例:

```
输入:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
输出: 6
```

解法：复用第84题的解法，从第一行开始求出每一行往上方的最大矩形，再在这些值中取最大

```java
public int maximalRectangle(char[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) {
        return 0;
    }
    int max = 0;

    int[] area = new int[matrix[0].length];
    int row = matrix.length;
    int line = matrix[0].length;
    for (int i = 0; i < row; i++) {
        for (int j = 0; j < line; j++) {
            if (matrix[i][j] == '1') {
                // 从之前的行下移一行，高度+1
                area[j] = area[j] + 1;
            } else {
                area[j] = 0;
            }
        }
        max = Math.max(max, largestRectangleArea(area));
    }
    return max;
}
```

#### [496. 下一个更大元素 I](https://leetcode-cn.com/problems/next-greater-element-i/)

给定两个 没有重复元素 的数组 nums1 和 nums2 ，其中nums1 是 nums2 的子集。找到 nums1 中每个元素在 nums2 中的下一个比其大的值。

nums1 中数字 x 的下一个更大元素是指 x 在 nums2 中对应位置的右边的第一个比 x 大的元素。如果不存在，对应位置输出 -1 。

 

示例 1:

```
输入: nums1 = [4,1,2], nums2 = [1,3,4,2].
输出: [-1,3,-1]
解释:
    对于num1中的数字4，你无法在第二个数组中找到下一个更大的数字，因此输出 -1。
    对于num1中的数字1，第二个数组中数字1右边的下一个较大数字是 3。
    对于num1中的数字2，第二个数组中没有下一个更大的数字，因此输出 -1。
```


示例 2:

```
输入: nums1 = [2,4], nums2 = [1,2,3,4].
输出: [3,-1]
解释:
    对于 num1 中的数字 2 ，第二个数组中的下一个较大数字是 3 。
    对于 num1 中的数字 4 ，第二个数组中没有下一个更大的数字，因此输出 -1 。
```


提示：

nums1和nums2中所有元素是唯一的。
nums1和nums2 的数组大小都不超过1000。

解法：单调递增栈+哈希表

```java
public int[] nextGreaterElement(int[] nums1, int[] nums2) {
    Stack<Integer> stack = new Stack<>();
    Map<Integer, Integer> map = new HashMap<>();

    for (int i = 0; i < nums2.length; i++) {
        while (!stack.isEmpty() && nums2[stack.peek()] < nums2[i]) {
            map.put(nums2[stack.pop()], nums2[i]);
        }
        stack.push(i);
    }

    int[] ret = new int[nums1.length];
    for (int i = 0; i < nums1.length; i++) {
        Integer value = map.get(nums1[i]);
        ret[i] = value == null ? -1 : value;
    }
    return ret;
}
```
#### [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

请根据每日 气温 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 0 来代替。

例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

提示：气温 列表长度的范围是 [1, 30000]。每个气温的值的均为华氏度，都是在 [30, 100] 范围内的整数。

解法：单调递增栈，类似于84题的 right数组，pop时得到结果

```java
public int[] dailyTemperatures(int[] T) {
    int[] ret = new int[T.length];
    Stack<Integer> stack = new Stack<>();
    for (int i = 0; i < T.length; i++) {
        while (!stack.isEmpty() && T[stack.peek()] < T[i]) {
            int popIndex = stack.pop();
            ret[popIndex] = i - popIndex;
        }
        stack.push(i);
    }
    while (!stack.isEmpty()) {
        int popIndex = stack.pop();
        ret[popIndex] = 0;
    }
    return ret;
}
```
#### [901. 股票价格跨度](https://leetcode-cn.com/problems/online-stock-span/)

编写一个 StockSpanner 类，它收集某些股票的每日报价，并返回该股票当日价格的跨度。

今天股票价格的跨度被定义为股票价格小于或等于今天价格的最大连续日数（从今天开始往回数，包括今天）。

例如，如果未来7天股票的价格是 [100, 80, 60, 70, 60, 75, 85]，那么股票跨度将是 [1, 1, 1, 2, 1, 4, 6]。

示例：

```
输入：["StockSpanner","next","next","next","next","next","next","next"], [[],[100],[80],[60],[70],[60],[75],[85]]
输出：[null,1,1,1,2,1,4,6]
解释：
首先，初始化 S = StockSpanner()，然后：
S.next(100) 被调用并返回 1，
S.next(80) 被调用并返回 1，
S.next(60) 被调用并返回 1，
S.next(70) 被调用并返回 2，
S.next(60) 被调用并返回 1，
S.next(75) 被调用并返回 4，
S.next(85) 被调用并返回 6。

注意 (例如) S.next(75) 返回 4，因为截至今天的最后 4 个价格
(包括今天的价格 75) 小于或等于今天的价格。
```


提示：

调用 StockSpanner.next(int price) 时，将有 1 <= price <= 10^5。
每个测试用例最多可以调用  10000 次 StockSpanner.next。
在所有测试用例中，最多调用 150000 次 StockSpanner.next。
此问题的总时间限制减少了 50%。

解法：单调递减栈，额外维护一个跨度栈

```java
class StockSpanner {

    Stack<Integer> prices, weights;

    public StockSpanner() {
        prices = new Stack();
        weights = new Stack();
    }

    public int next(int price) {
        int w = 1;
        while (!prices.isEmpty() && prices.peek() <= price) {
            prices.pop();
            w += weights.pop();
        }

        prices.push(price);
        weights.push(w);
        return w;
    }
}
```