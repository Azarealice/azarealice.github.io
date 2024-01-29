---
categories: 技术 - 算法
tags: 技术
date: 2021/1/30 12:45:00
---

### LeetCode - 进阶 - 树状数组

#### [315. 计算右侧小于当前元素的个数](https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/)

给定一个整数数组 nums，按要求返回一个新数组 counts。数组 counts 有该性质： counts[i] 的值是  nums[i] 右侧小于 nums[i] 的元素的数量。

示例：

```
输入：nums = [5,2,6,1]
输出：[2,1,1,0] 
解释：
5 的右侧有 2 个更小的元素 (2 和 1)
2 的右侧仅有 1 个更小的元素 (1)
6 的右侧有 1 个更小的元素 (1)
1 的右侧有 0 个更小的元素
```

<!--more-->

解法：从右往左遍历，用树状数组记录每个数字出现的次数。

```java
class Solution {

    public List<Integer> countSmaller(int[] nums) {
        int max = nums[0];
        for (int n : nums) {
            max = Math.max(max, n);
        }
        int mask = 10001;
        BIT bit = new BIT(max + mask);

        LinkedList<Integer> list = new LinkedList<>();
        for (int i = nums.length - 1; i >= 0; i--) {
            bit.update(nums[i] + mask);
            list.addFirst(bit.query(nums[i] + mask - 1));
        }
        return list;
    }

    public class BIT {

        int n;
        int[] arr;

        public BIT(int n) {
            this.n = n;
            arr = new int[n + 1];
        }

        public void update(int x) {
            while (x <= n) {
                arr[x]++;
                x += x & -x;
            }
        }

        public int query(int x) {
            int res = 0;
            while (x >= 1) {
                res += arr[x];
                x -= x & -x;
            }
            return res;
        }
    }
}
```

#### [1505. 最多 K 次交换相邻数位后得到的最小整数](https://leetcode-cn.com/problems/minimum-possible-integer-after-at-most-k-adjacent-swaps-on-digits/)

给你一个字符串 num 和一个整数 k 。其中，num 表示一个很大的整数，字符串中的每个字符依次对应整数上的各个 数位 。

你可以交换这个整数相邻数位的数字 最多 k 次。

请你返回你能得到的最小整数，并以字符串形式返回。

示例 1：

```
输入：num = "4321", k = 4
输出："1342"
解释：4321 通过 4 次交换相邻数位得到最小整数的步骤如上图所示。
```


示例 2：

```
输入：num = "100", k = 1
输出："010"
解释：输出可以包含前导 0 ，但输入保证不会有前导 0 。
```


示例 3：

```
输入：num = "36789", k = 1000
输出："36789"
解释：不需要做任何交换。
```


示例 4：

```
输入：num = "22", k = 22
输出："22"
```


示例 5：

```
输入：num = "9438957234785635408", k = 23
输出："0345989723478563548"
```


提示：

```
1 <= num.length <= 30000
num 只包含 数字 且不含有 前导 0 。
1 <= k <= 10^9
```

<!--more-->

解法：从左向右填入尽可能小的数字，用树状数组记录被替换位置后面的位置被使用的次数（behind），加到当前替换需要的步数中，因为后面的位置被替换过，代表原位置要后移。

```java
class Solution {
    public String minInteger(String num, int k) {
        int n = num.length();
        Queue<Integer>[] pos = new Queue[10];
        for (int i = 0; i < 10; ++i) {
            pos[i] = new LinkedList<Integer>();
        }
        for (int i = 0; i < n; ++i) {
            pos[num.charAt(i) - '0'].offer(i + 1);
        }
        StringBuffer ans = new StringBuffer();
        BIT bit = new BIT(n);
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 10; ++j) {
                if (!pos[j].isEmpty()) {
                    int behind = bit.query(pos[j].peek() + 1, n);
                    int dist = pos[j].peek() + behind - i;
                    if (dist <= k) {
                        bit.update(pos[j].poll());
                        ans.append(j);
                        k -= dist;
                        break;
                    }
                }
            }
        }
        return ans.toString();
    }
}

class BIT {
    int n;
    int[] tree;

    public BIT(int n) {
        this.n = n;
        this.tree = new int[n + 1];
    }

    public static int lowbit(int x) {
        return x & (-x);
    }

    public void update(int x) {
        while (x <= n) {
            ++tree[x];
            x += lowbit(x);
        }
    }

    public int query(int x) {
        int ans = 0;
        while (x > 0) {
            ans += tree[x];
            x -= lowbit(x);
        }
        return ans;
    }

    public int query(int x, int y) {
        return query(y) - query(x - 1);
    }
}
```

