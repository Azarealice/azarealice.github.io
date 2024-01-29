---
categories: 技术 - 算法
tags: 技术
date: 2021/1/4 11:40:00

---

### LeetCode - 位运算

#### Cheet Sheet

```java
// rightmost 1-bit in x
//  x = 0000001100
// -x = 1111110110
//  d = 0000000100
int y = Integer.lowestOneBit(x);

public static int lowestOneBit(int i) {
    // HD, Section 2-1
    return i & -i;
}

// leftmost 1-bit in x
int y = Integer.highestOneBit(x);

public static int highestOneBit(int i) {
    // HD, Figure 3-1
    i |= (i >>  1);   		// 000011000000...000
    i |= (i >>  2);   		// 000011110000...000
    i |= (i >>  4);   		// 000011111111...000
    i |= (i >>  8);   		// 000011111111...000
    i |= (i >> 16);   		// 000011111111...111
    return i - (i >>> 1); // 000010000000...000
}

// get number of one-bits of x
// bitCount of -1 int is 32
int bitCount = Integer.bitCount(x);

// Brian Kernighan 算法 将x的最右端的1变成0
int y = x & (x-1)
  
// 枚举000101101比特位子集
// output 000101101,000101100,000101001,000101000,000100101...
for (int y = x; y > 0; y = x & (y - 1))

// 枚举000101101的单比特位子集
// output 000000001,000000100,000001000,000100000
for (int mask = x, pp; mask > 0; mask ^= pp) {
    pp = mask & -mask;
}
```

#### [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

```
输入: [2,2,1]
输出: 1
```


示例 2:

```
输入: [4,1,2,1,2]
输出: 4
```

<!--more-->

解法：异或满足交换律，可得4 ^ 1 ^ 2 ^ 1 ^ 2 = 4 ^ (1 ^ 1) ^ (2 ^ 2) = 4

```java
public int singleNumber(int[] nums) {
    int single = 0;
    for (int num : nums) {
        single ^= num;
    }
    return single;
}
```

#### [137. 只出现一次的数字 II](https://leetcode-cn.com/problems/single-number-ii/)

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现了三次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

```
输入: [2,2,3,2]
输出: 3
```


示例 2:

```
输入: [0,1,0,1,0,1,99]
输出: 99
```

解法：


```java
public int singleNumber(int[] nums) {
    int ans = 0;
    //考虑每一位
    for (int i = 0; i < 32; i++) {
        int count = 0;
        //考虑每一个数
        for (int j = 0; j < nums.length; j++) {
            //当前位是否是 1
            if ((nums[j] >>> i & 1) == 1) {
                count++;
            }
        }
        //1 的个数是否是 3 的倍数
        if (count % 3 != 0) {
            ans += 1 << i;
        }
    }
    return ans;
}

public int singleNumber(int[] nums) {
    int seenOnce = 0, seenTwice = 0;

    for (int num : nums) {
        // first appearence:
        // add num to seen_once
        // don't add to seen_twice because of presence in seen_once

        // second appearance:
        // remove num from seen_once
        // add num to seen_twice

        // third appearance:
        // don't add to seen_once because of presence in seen_twice
        // remove num from seen_twice
        seenOnce = ~seenTwice & (seenOnce ^ num);
        seenTwice = ~seenOnce & (seenTwice ^ num);
    }

    return seenOnce;
}
```

#### [260. 只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/)

给定一个整数数组 nums，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。

示例 :

```
输入: [1,2,1,3,2,5]
输出: [3,5]
```


注意：

结果输出的顺序并不重要，对于上面的例子， [5, 3] 也是正确答案。
你的算法应该具有线性时间复杂度。你能否仅使用常数空间复杂度来实现？

解法：

```java
public int[] singleNumber(int[] nums) {
    // difference between two numbers (x and y) which were seen only once
    int bitmask = 0;
    for (int num : nums) bitmask ^= num;

    // rightmost 1-bit diff between x and y
    int diff = bitmask & (-bitmask);

    int x = 0;
    // bitmask which will contain only x
    for (int num : nums) if ((num & diff) != 0) x ^= num;

    return new int[]{x, bitmask^x};
}
```

#### [1125. 最小的必要团队](https://leetcode-cn.com/problems/smallest-sufficient-team/)

作为项目经理，你规划了一份需求的技能清单 req_skills，并打算从备选人员名单 people 中选出些人组成一个「必要团队」（ 编号为 i 的备选人员 people[i] 含有一份该备选人员掌握的技能列表）。

所谓「必要团队」，就是在这个团队中，对于所需求的技能列表 req_skills 中列出的每项技能，团队中至少有一名成员已经掌握。

我们可以用每个人的编号来表示团队中的成员：例如，团队 team = [0, 1, 3] 表示掌握技能分别为 people[0]，people[1]，和 people[3] 的备选人员。

请你返回 任一 规模最小的必要团队，团队成员用人员编号表示。你可以按任意顺序返回答案，本题保证答案存在。

 

示例 1：

```
输入：req_skills = ["java","nodejs","reactjs"], people = [["java"],["nodejs"],["nodejs","reactjs"]]
输出：[0,2]
```


示例 2：

```
输入：req_skills = ["algorithms","math","java","reactjs","csharp","aws"], people = [["algorithms","math","java"],["algorithms","math","reactjs"],["java","csharp","aws"],["reactjs","csharp"],["csharp","math"],["aws","java"]]
输出：[1,2]
```


提示：

```
1 <= req_skills.length <= 16
1 <= people.length <= 60
1 <= people[i].length, req_skills[i].length, people[i][j].length <= 16
req_skills 和 people[i] 中的元素分别各不相同
req_skills[i][j], people[i][j][k] 都由小写英文字母组成
本题保证「必要团队」一定存在
```

解法：

```java
class Solution {

    //成员技能对应二进制数字都数组下标
    private int[] peopleSubscript;

    //当前递归深度
    private int currentDepth = 0;

    public int[] smallestSufficientTeam(String[] req_skills, List<List<String>> people) {
        int[] result = null;
        int skillLen = req_skills.length, mustSkill = (1 << skillLen) - 1, peopleSize = people.size();
        //技能对应二进制，用1，10，100来表示
        Map<String, Integer> skillMap = new HashMap<String, Integer>(skillLen << 1);
        for (int i = 0; i < skillLen; i++) {
            skillMap.put(req_skills[i], 1 << i);
        }
        //员工技能对应二进制数
        int[] peopleSkillNums = new int[peopleSize];
        peopleSubscript = new int[1 << skillLen];
        for (int i = 0; i < people.size(); i++) {
            int skillNum = 0;
            List<String> skills = people.get(i);
            for (String skill : skills) {
                skillNum += skillMap.get(skill);
            }
            peopleSkillNums[i] = skillNum;
            peopleSubscript[skillNum] = i;
        }
        //技能或运算结果
        int comSkills = 0;
        //和其他所有员工没交集对应员工数量
        int aloneCount = 0;
        //和其他所有员工没交集对应员工数组下标
        int[] noIntersectionArr = new int[skillLen];
        for (int i = 0; i < peopleSize; i++) {
            if (peopleSkillNums[i] == 0) {
                continue;
            }
            //是否无交集
            boolean isNoIntersection = true;
            for (int j = 0; j < peopleSize; j++) {
                if (peopleSkillNums[j] == 0 || i == j) {
                    continue;
                }
                //重复的，去重
                if (peopleSkillNums[i] == peopleSkillNums[j]) {
                    peopleSkillNums[j] = 0;
                    continue;
                }
                //若一个员工技能是另外一个员工子集，则必定不在最优解中，去除
                if ((peopleSkillNums[i] | peopleSkillNums[j]) == peopleSkillNums[j]) {
                    peopleSkillNums[i] = 0;
                    isNoIntersection = false;
                    break;
                } else if ((peopleSkillNums[i] | peopleSkillNums[j]) == peopleSkillNums[i]) {
                    peopleSkillNums[j] = 0;
                    continue;
                }
                if ((peopleSkillNums[i] & peopleSkillNums[j]) != 0) {
                    isNoIntersection = false;
                    break;
                }
            }
            //无交集员工提前保存，方便之后回溯（降低后续回溯深度）
            if (isNoIntersection) {
                comSkills |= peopleSkillNums[i];
                noIntersectionArr[aloneCount] = i;
                peopleSkillNums[i] = 0;
                aloneCount++;
            }
        }
        //员工技能数字排序
        Arrays.sort(peopleSkillNums);
        //最小回溯深度，由小到大，则第一个得到结果就为最优解
        int minDepth = aloneCount;
        //若无交集员工技能组成等于必须技能，则输出结果，否则开始回溯深度+1
        if (comSkills == mustSkill) {
            result = new int[aloneCount];
            System.arraycopy(noIntersectionArr, 0, result, 0, aloneCount);
            return result;
        } else {
            minDepth++;
        }
        //从minDepth回溯深度开始回溯，noIntersectionArr肯定在该结果中，回溯深度从无交集员工数量开始
        for (int i = minDepth; i < skillLen; i++) {
            currentDepth = i;
            result = new int[i];
            System.arraycopy(noIntersectionArr, 0, result, 0, aloneCount);
            if (addNextPeople(mustSkill, comSkills, result, peopleSkillNums, aloneCount)) {
                break;
            }
        }
        return result;
    }

    private boolean addNextPeople(int mustSkill, int comSkills, int[] result, int[] peopleSkillNums, int count) {
        //判断是否为解
        if (mustSkill == comSkills) {
            return true;
        }
        //大于回溯深度，则不存在
        if (count >= currentDepth) {
            return false;
        }
        for (int i = peopleSkillNums.length - 1; i >= 0; i--) {
            int skillNum = peopleSkillNums[i];
            //由于排序，则技能为0则后续都为0，直接结束
            if (skillNum == 0) {
                break;
            }
            //组合技能已包含该技能，则跳过
            if ((comSkills | skillNum) == comSkills) {
                continue;
            }
            result[count] = peopleSubscript[peopleSkillNums[i]];
            peopleSkillNums[i] = 0;
            if (addNextPeople(mustSkill, comSkills | skillNum, result, peopleSkillNums, count + 1)) {
                return true;
            }
            peopleSkillNums[i] = skillNum;
        }
        return false;
    }
}
```
#### [1723. 完成所有工作的最短时间](https://leetcode-cn.com/problems/find-minimum-time-to-finish-all-jobs/)

给你一个整数数组 jobs ，其中 jobs[i] 是完成第 i 项工作要花费的时间。

请你将这些工作分配给 k 位工人。所有工作都应该分配给工人，且每项工作只能分配给一位工人。工人的 工作时间 是完成分配给他们的所有工作花费时间的总和。请你设计一套最佳的工作分配方案，使工人的 最大工作时间 得以 最小化 。

返回分配方案中尽可能 最小 的 最大工作时间 。

 

示例 1：

```
输入：jobs = [3,2,3], k = 3
输出：3
解释：给每位工人分配一项工作，最大工作时间是 3 。
```


示例 2：

```
输入：jobs = [1,2,4,7,8], k = 2
输出：11
解释：按下述方式分配工作：
1 号工人：1、2、8（工作时间 = 1 + 2 + 8 = 11）
2 号工人：4、7（工作时间 = 4 + 7 = 11）
最大工作时间是 11 。
```


提示：

```
1 <= k <= jobs.length <= 12
1 <= jobs[i] <= 107
```

解法：按位预处理所有子集工作时间总和，二分查找最大工作时间。

```java
int[] subSum;
int[] jobs;
int k;
int n;

public int minimumTimeRequired(int[] jobs, int k) {
    this.jobs = jobs;
    this.k = k;
    this.n = jobs.length;
    int sum = 0;
    Arrays.sort(jobs);

    for (int i = 0; i < n; i++) {
        sum += jobs[i];
    }
    int l = sum / k;
    int r = sum;

    subSum = new int[1 << n];
    for (int i = 0; i < (1 << n); i++) {
        for (int j = 0; j < n; j++) {
            if ((i & (1 << j)) > 0) {
                subSum[i] += jobs[j];
            }
        }
    }

    while (l < r) {
        int m = l + (r - l) / 2;
        if (check(m)) {
            r = m;
        } else {
            l = m + 1;
        }
    }

    return l;
}

private boolean check(int m) {
    int[] dp = new int[1 << n];

    for (int i = 1; i < 1 << n; i++) {
        for (int sub = i; sub > 0; sub = (sub - 1) & i) {
            dp[i] = k + 1;
            if (subSum[sub] <= m) {
                dp[i] = Math.min(dp[i], dp[i ^ sub] + 1);
            }else{
                break;
            }
        }
    }
    return dp[(1 << n) - 1] <= k;
}
```

