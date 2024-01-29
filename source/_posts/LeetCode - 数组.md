---
categories: 技术 - 算法
tags: 技术
date: 2020/7/22 22:28:00
---

### LeetCode - 数组

#### [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。

请你找出这两个正序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。



示例 1:

```
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```

示例 2:

```
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

<!--more-->

解法：在两个数组之中二分查找第k小的数即可

存在更优解法：使时间复杂度降为 O(log(min(m + n)))，参考：https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-w-2/

```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    int n = nums1.length;
    int m = nums2.length;
    int left = (n + m + 1) / 2;
    int right = (n + m + 2) / 2;
    //将偶数和奇数的情况合并，如果是奇数，会求两次同样的 k 。
    return (getKth(nums1, 0, n - 1, nums2, 0, m - 1, left) + getKth(nums1, 0, n - 1, nums2, 0, m - 1, right)) * 0.5;  
}

private int getKth(int[] nums1, int start1, int end1, int[] nums2, int start2, int end2, int k) {
      int len1 = end1 - start1 + 1;
      int len2 = end2 - start2 + 1;
      //让 len1 的长度小于 len2，这样就能保证如果有数组空了，一定是 len1 
      if (len1 > len2) return getKth(nums2, start2, end2, nums1, start1, end1, k);
      if (len1 == 0) return nums2[start2 + k - 1];

      if (k == 1) return Math.min(nums1[start1], nums2[start2]);

      int i = start1 + Math.min(len1, k / 2) - 1;
      int j = start2 + Math.min(len2, k / 2) - 1;

      if (nums1[i] > nums2[j]) {
      return getKth(nums1, start1, end1, nums2, j + 1, end2, k - (j - start2 + 1));
      }
      else {
      return getKth(nums1, i + 1, end1, nums2, start2, end2, k - (i - start1 + 1));
      }
}
```

#### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

 

示例：

```
给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

暴力遍历的复杂度为O(n^3)，但在选定第一个元素之后使用双指针寻找剩下的两个元素，可将复杂度降低到O(n^2)

```java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> ret = new ArrayList<>();
    Arrays.sort(nums);
    int len = nums.length;
    for (int i = 0; i < len - 2; i++) {
        if (nums[i] > 0) {
            break;
        }
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;
        }
        int left = i + 1;
        int right = len - 1;

        int x = -nums[i];

        while (left < right) {
            if (nums[left] + nums[right] == x) {
                ret.add(Arrays.asList(nums[i], nums[left], nums[right]));
                left++;
                right--;
                while (left < right && nums[left] == nums[left - 1]) {
                    left++;
                }
                while (left < right && nums[right] == nums[right + 1]) {
                    right--;
                }
            } else if (nums[left] + nums[right] < x) {
                left++;
            } else {
                right--;
            }
        }
    }
    return ret;
}
```

#### [31. 下一个排列](https://leetcode-cn.com/problems/next-permutation/)

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须原地修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。

```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

解法：（此方法被收录进c++标准库中，next_permutation）此处以12354321为例进行说明，首先从右往左寻找第一个比右边元素小的位置i，在这里i=2（值为3），寻找到i之后，需要找到比i大的最右位置，即j，这里为j=4(值为4)，将i和j互换，再将i之后的数组反转即可，答案为12412335。

```java
public void nextPermutation(int[] nums) {
    int n = nums.length;
    for (int i = n - 2; i >= 0; i--) {
        if (nums[i] < nums[i + 1]) {
            int j = n - 1;
            while (nums[j] <= nums[i]) {
                j--;
            }
            exchange(nums, i, j);
            reverse(nums, i + 1, n - 1);
            return;
        }
    }
    reverse(nums, 0, n - 1);
}

private void exchange(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}

private void reverse(int[] nums, int lo, int hi) {
    while (lo < hi) {
        exchange(nums, lo++, hi--);
    }
}
```

#### [41. 缺失的第一个正数](https://leetcode-cn.com/problems/first-missing-positive/)

给你一个未排序的整数数组，请你找出其中没有出现的最小的正整数。

 

示例 1:

```
输入: [1,2,0]
输出: 3
```


示例 2:

```
输入: [3,4,-1,1]
输出: 2
```


示例 3:

```
输入: [7,8,9,11,12]
输出: 1
```


提示：

你的算法的时间复杂度应为O(n)，并且只能使用常数级别的额外空间。

解法：难点在于空间复杂度要求O(1)，也就是要使用原数组表示更多的信息，考虑使用正负值表示出没出现过，由于有正负翻转绝对值保持不变的特性，绝对值不会被破坏，用负数代表未出现，正数代表出现过了。一开始先将原有负数处理成无效值，后面使用绝对值作为下标翻转数组正负值即可。

```java
public int firstMissingPositive(int[] nums) {

    int len = nums.length;

    for (int i = 0; i < len; i++) {
        if (nums[i] <= 0) {
            nums[i] = len + 1;
        }
    }

    for (int i = 0; i < len; i++) {
        if (Math.abs(nums[i]) <= len) {
            nums[Math.abs(nums[i]) - 1] = -Math.abs(nums[Math.abs(nums[i]) - 1]);
        }
    }

    for (int i = 0; i < len; i++) {
        if (nums[i] > 0) {
            return i + 1;
        }
    }
    return len + 1;
}
```

#### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

```
输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

非常经典的一道题，遍历向前，小于0的子序列全部丢弃即可
```java
public int maxSubArray(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    int max = nums[0];
    int ans = 0;
    for (int x : nums) {
        ans += x;
        max = Math.max(max, ans);
        if (ans < 0) {
            ans = 0;
        }
    }
    return max;
}
```

#### [229. 求众数 II](https://leetcode-cn.com/problems/majority-element-ii/)

给定一个大小为 n 的整数数组，找出其中所有出现超过 ⌊ n/3 ⌋ 次的元素。

进阶：尝试设计时间复杂度为 O(n)、空间复杂度为 O(1)的算法解决此问题。

 

示例 1：

```
输入：[3,2,3]
输出：[3]
```


示例 2：

```
输入：nums = [1]
输出：[1]
```


示例 3：

```
输入：[1,1,1,3,3,2,2,2]
输出：[1,2]
```


提示：

1 <= nums.length <= 5 * 104
-109 <= nums[i] <= 109

解法：摩尔投票法，核心思想是对拼消耗

```java
public List<Integer> majorityElement(int[] nums) {
    int maj1 = 0, maj2 = 0;
    int cnt1 = 0, cnt2 = 0;

    for (int x : nums) {
        if (maj1 == x) {
            cnt1++;
        } else if (maj2 == x) {
            cnt2++;
        } else if (cnt1 == 0) {
            maj1 = x;
            cnt1++;
        } else if (cnt2 == 0) {
            maj2 = x;
            cnt2++;
        } else {
            cnt1--;
            cnt2--;
        }
    }

    cnt1 = 0;
    cnt2 = 0;

    for (int x : nums) {
        if (maj1 == x) {
            cnt1++;
        }
        if (maj2 == x) {
            cnt2++;
        }
    }

    List<Integer> ret = new ArrayList<>();
    if (cnt1 > nums.length / 3) {
        ret.add(maj1);
    }
    if (cnt2 > nums.length / 3 && maj1 != maj2) {
        ret.add(maj2);
    }
    return ret;
}
```

#### [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。


示例 1：

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```


示例 2：

```
输入：nums = [0,1,0,3,2,3]
输出：4
```


示例 3：

```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```


提示：

```
1 <= nums.length <= 2500
-104 <= nums[i] <= 104
```


进阶：

你可以设计时间复杂度为 O(n^2) 的解决方案吗？
你能将算法的时间复杂度降低到 O(n log(n)) 吗?

解法：两种解法，常规解法动态规划复杂度为O(n^2)，贪心+二分查找为O(n log(n))，维护一个数组 d[i] ，表示长度为 i 的最长上升子序列的末尾元素的最小值，用 len 记录目前最长上升子序列的长度，起始时 len 为 1，d[1]=nums[0]，并且d[i] 是关于 i 单调递增的

```
以输入序列 [0, 8, 4, 12, 2] 为例：

第一步插入 0，d = [0]
第二步插入 8，d = [0, 8]
第三步插入 4，d = [0, 4]
第四步插入 12，d = [0, 4, 12]
第五步插入 2，d = [0, 2, 12]
```

```java
public int lengthOfLIS(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    int[] dp = new int[nums.length];
    dp[0] = 1;
    int maxans = 1;
    for (int i = 1; i < nums.length; i++) {
        dp[i] = 1;
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        maxans = Math.max(maxans, dp[i]);
    }
    return maxans;
}

public int lengthOfLIS(int[] nums) {
    int len = 1, n = nums.length;
    if (n == 0) {
        return 0;
    }
    int[] d = new int[n + 1];
    d[len] = nums[0];
    for (int i = 1; i < n; ++i) {
        if (nums[i] > d[len]) {
            d[++len] = nums[i];
        } else {
            int l = 1;
            int r = len;
            //寻找大于等于nums[i]的最小下标
            while (l < r) {
                int mid = l + (r - l) / 2;
                if (d[mid] < nums[i]) {
                    l = mid + 1;
                } else {
                    r = mid;
                }
            }
            d[l] = nums[i];
        }
    }
    return len;
}
```

#### [313. 超级丑数](https://leetcode-cn.com/problems/super-ugly-number/)

编写一段程序来查找第 n 个超级丑数。

超级丑数是指其所有质因数都是长度为 k 的质数列表 primes 中的正整数。

示例:

```
输入: n = 12, primes = [2,7,13,19]
输出: 32 
解释: 给定长度为 4 的质数列表 primes = [2,7,13,19]，前 12 个超级丑数序列为：[1,2,4,7,8,13,14,16,19,26,28,32] 。
```


说明:

1 是任何给定 primes 的超级丑数。
 给定 primes 中的数字以升序排列。
0 < k ≤ 100, 0 < n ≤ 106, 0 < primes[i] < 1000 。
第 n 个超级丑数确保在 32 位有符整数范围内。

解法：第一个数为1，第二个数为(1\*2,1\*7,1\*13,1\*19)的最小值，第三个数为(2\*2,1\*7,1\*13,1\*19)的最小值，第四个数为(4\*2,1\*7,1\*13,1\*19)的最小值，第四个数为(4\*2,2\*7,1\*13,1\*19)的最小值

```java
public int nthSuperUglyNumber(int n, int[] primes) {
    int[] nums = new int[n];
    int[] c = new int[primes.length];

    nums[0] = 1;
    for (int i = 1; i < n; i++) {
        int min = Integer.MAX_VALUE;
        int index = -1;
        for (int j = 0; j < c.length; j++) {
            if (nums[c[j]] * primes[j] < min) {
                min = nums[c[j]] * primes[j];
                index = j;
            } else if (nums[c[j]] * primes[j] == min) {
                c[j]++;
            }
        }

        nums[i] = min;
        c[index]++;
    }
    return nums[n - 1];
}
```

#### [1124. 表现良好的最长时间段](https://leetcode-cn.com/problems/longest-well-performing-interval/)

给你一份工作时间表 hours，上面记录着某一位员工每天的工作小时数。

我们认为当员工一天中的工作小时数大于 8 小时的时候，那么这一天就是「劳累的一天」。

所谓「表现良好的时间段」，意味在这段时间内，「劳累的天数」是严格 大于「不劳累的天数」。

请你返回「表现良好时间段」的最大长度。 

示例 1：

```
输入：hours = [9,9,6,0,6,6,9]
输出：3
解释：最长的表现良好时间段是 [9,9,6]。
```


提示：

```
1 <= hours.length <= 10000
0 <= hours[i] <= 16
```

```java
public int longestWPI(int[] hours) {
    Map<Integer, Integer> first = new HashMap<>();

    int cnt = 0;
    int ret = 0;

    for (int i = 0; i < hours.length; i++) {
        if (hours[i] > 8) {
            cnt++;
        } else {
            cnt--;
        }
        if (!first.containsKey(cnt)) {
            first.put(cnt, i);
        }
        if (cnt > 0) {
            ret = i + 1;
        } else {
            Integer pos = first.get(cnt - 1);
            if (pos != null) {
                ret = Math.max(i - pos, ret);
            }
        }
    }

    return ret;
}
```

#### [1674. 使数组互补的最少操作次数](https://leetcode-cn.com/problems/minimum-moves-to-make-array-complementary/)

给你一个长度为 偶数 n 的整数数组 nums 和一个整数 limit 。每一次操作，你可以将 nums 中的任何整数替换为 1 到 limit 之间的另一个整数。

如果对于所有下标 i（下标从 0 开始），nums[i] + nums[n - 1 - i] 都等于同一个数，则数组 nums 是 互补的 。例如，数组 [1,2,3,4] 是互补的，因为对于所有下标 i ，nums[i] + nums[n - 1 - i] = 5 。

返回使数组 互补 的 最少 操作次数。

 

示例 1：

```
输入：nums = [1,2,4,3], limit = 4
输出：1
解释：经过 1 次操作，你可以将数组 nums 变成 [1,2,2,3]（加粗元素是变更的数字）：
nums[0] + nums[3] = 1 + 3 = 4.
nums[1] + nums[2] = 2 + 2 = 4.
nums[2] + nums[1] = 2 + 2 = 4.
nums[3] + nums[0] = 3 + 1 = 4.
对于每个 i ，nums[i] + nums[n-1-i] = 4 ，所以 nums 是互补的。
```


示例 2：

```
输入：nums = [1,2,2,1], limit = 2
输出：2
解释：经过 2 次操作，你可以将数组 nums 变成 [2,2,2,2] 。你不能将任何数字变更为 3 ，因为 3 > limit 。
```


示例 3：

```
输入：nums = [1,2,1,2], limit = 2
输出：0
解释：nums 已经是互补的。
```

解法：差分数组，考虑任意一个数对(a,b)，不妨假设a≤b。假设最终选定的和值为target，则我们可以发现，对于(a,b)这个数对：

当target<1+a时，需要操作两次；
当1+a<=target<a+b时，需要操作一次；
当target=a+b时，不需要操作；
当a+b<target<=b+limit时，需要操作一次；
当target>b+limit时，需要操作两次。

我们设初始操作次数为两次。令target从数轴最左端开始向右移动，我们会发现：

在1+a处，操作次数减少一次；
在a+b处，操作次数减少一次；
在a+b+1处，操作次数增加一次；
在b+limit+1处，操作次数增加一次。
因此，我们可以遍历数组中的所有数对，求出每个数对的这四个关键位置，然后更新差分数组。最后，我们遍历（扫描）差分数组，就可以找到令总操作次数最小的target，同时可以得到最少的操作次数。

```java
public int minMoves(int[] nums, int limit) {
    int len = nums.length;

    int[] res = new int[2 * limit + 2]; // max res index is 2 * limit + 1

    for (int i = 0; i < len / 2; i++) {

        int left = 1 + Math.min(nums[i], nums[len - 1 - i]);
        int sum = nums[i] + nums[len - 1 - i];
        int right = limit + Math.max(nums[i], nums[len - 1 - i]);

        res[2] = res[2] + 2;
        res[left]--;
        res[sum]--;
        res[sum + 1]++;
        res[right + 1]++;
    }

    int minMoves = 2 * len;
    int curMoves = 0;

    for (int i = 2; i <= 2 * limit; i++) {
        curMoves += res[i];
        minMoves = Math.min(minMoves, curMoves);
    }
    return minMoves;
}
```