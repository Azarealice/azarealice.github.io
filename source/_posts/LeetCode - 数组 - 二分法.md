---
categories: 技术 - 算法
tags: 技术
date: 2021/1/3 15:30:00
---

### LeetCode - 数组 - 二分法

#### [658. 找到 K 个最接近的元素](https://leetcode-cn.com/problems/find-k-closest-elements/)

给定一个排序好的数组 arr ，两个整数 k 和 x ，从数组中找到最靠近 x（两数之差最小）的 k 个数。返回的结果必须要是按升序排好的。

整数 a 比整数 b 更接近 x 需要满足：

|a - x| < |b - x| 或者
|a - x| == |b - x| 且 a < b


示例 1：

```
输入：arr = [1,2,3,4,5], k = 4, x = 3
输出：[1,2,3,4]
```


示例 2：

```
输入：arr = [1,2,3,4,5], k = 4, x = -1
输出：[1,2,3,4]
```


提示：

```
1 <= k <= arr.length
1 <= arr.length <= 104
数组里的每个元素与 x 的绝对值不超过 104
```

<!--more-->

```java
public List<Integer> findClosestElements(int[] arr, int k, int x) {
    int l = 0, r = arr.length - k;
    while (l < r) {
        int mid = l + (r - l) / 2;
        if (x - arr[mid] > arr[mid + k] - x) {
            l = mid + 1;
        } else {
            r = mid;
        }
    }
    //以l作为起始索引，长度为k
    List<Integer> res = new LinkedList<>();
    for (int i = 0; i < k; i++) {
        res.add(arr[i + l]);
    }
    return res;

}
```

#### [719. 找出第 k 小的距离对](https://leetcode-cn.com/problems/find-k-th-smallest-pair-distance/)

给定一个整数数组，返回所有数对之间的第 k 个最小距离。一对 (A, B) 的距离被定义为 A 和 B 之间的绝对差值。

示例 1:

```
输入：
nums = [1,3,1]
k = 1
输出：0 

解释：
所有数对如下：
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
```

因此第 1 个最小距离的数对是 (1,1)，它们之间的距离为 0。
提示:

```
2 <= len(nums) <= 10000.
0 <= nums[i] < 1000000.
1 <= k <= len(nums) * (len(nums) - 1) / 2.
```

解法：从0到最大距离之间进行二分枚举尝试，计算时采用双指针逼近法

```java
public int smallestDistancePair(int[] nums, int k) {
    Arrays.sort(nums);

    int lo = 0;
    int hi = nums[nums.length - 1] - nums[0];
    while (lo < hi) {
        int mi = (lo + hi) / 2;
        int count = 0, left = 0;
        for (int right = 0; right < nums.length; ++right) {
            while (nums[right] - nums[left] > mi) {
                left++;
            }
            count += right - left;
        }
        //count = number of pairs with distance <= mi
        if (count >= k) {
            hi = mi;
        } else {
            lo = mi + 1;
        }
    }
    return lo;
}
```

