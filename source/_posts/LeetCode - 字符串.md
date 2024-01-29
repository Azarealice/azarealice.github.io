---
categories: 技术 - 算法
tags: 技术
date: 2020/7/12 15:42:00
---

### LeetCode - 字符串

#### [14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

示例 1:

```
输入: ["flower","flow","flight"]
输出: "fl"
```


示例 2:

```
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```


说明:

所有输入只包含小写字母 a-z 

<!--more-->

解法：最长公共前缀必然来自于strs[0]的某个前面部分，从strs[1]开始遍历比较这个初始值，每次不符合时剪掉最后一位

```java
 public String longestCommonPrefix(String[] strs) {
    if (strs.length == 0) {
        return "";
    }
    String prefix = strs[0];
    for (int i = 1; i < strs.length; i++) {
        while (strs[i].indexOf(prefix) != 0) {
            prefix = prefix.substring(0, prefix.length() - 1);
            if (prefix.isEmpty()) {
                return "";
            }
        }
    }
    return prefix;
}
```

#### [316. 去除重复字母](https://leetcode-cn.com/problems/remove-duplicate-letters/)

给你一个仅包含小写字母的字符串，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证返回结果的字典序最小（要求不能打乱其他字符的相对位置）。

示例 1:

```
输入: "bcabc"
输出: "abc"
```


示例 2:

```
输入: "cbacdcbc"
输出: "acdb"
```

解法：记录每个字母最后出现的位置，从左往右顺序推进，当且仅当前一个字母字典序大并且该字母在后面位置还有出现时，才舍去该字母

```java
public String removeDuplicateLetters(String s) {

    Stack<Character> stack = new Stack<>();

    // this lets us keep track of what's in our solution in O(1) time
    HashSet<Character> seen = new HashSet<>();

    // this will let us know if there are any more instances of s[i] left in s
    HashMap<Character, Integer> last_occurrence = new HashMap<>();
    for (int i = 0; i < s.length(); i++) {
        last_occurrence.put(s.charAt(i), i);
    }

    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        // we can only try to add c if it's not already in our solution
        // this is to maintain only one of each character
        if (!seen.contains(c)) {
            // if the last letter in our solution:
            //     1. exists
            //     2. is greater than c so removing it will make the string smaller
            //     3. it's not the last occurrence
            // we remove it from the solution to keep the solution optimal
            while (!stack.isEmpty() && c < stack.peek() && last_occurrence.get(stack.peek()) > i) {
                seen.remove(stack.pop());
            }
            seen.add(c);
            stack.push(c);
        }
    }
    StringBuilder sb = new StringBuilder(stack.size());
    for (Character c : stack) {
        sb.append(c.charValue());
    }
    return sb.toString();
}
```

#### [385. 迷你语法分析器](https://leetcode-cn.com/problems/mini-parser/)

给定一个用字符串表示的整数的嵌套列表，实现一个解析它的语法分析器。

列表中的每个元素只可能是整数或整数嵌套列表

提示：你可以假定这些字符串都是格式良好的：

字符串非空
字符串不包含空格
字符串只包含数字0-9、[、-、,、]


示例 1：

```
给定 s = "324",

你应该返回一个 NestedInteger 对象，其中只包含整数值 324。
```

示例 2：

```
给定 s = "[123,[456,[789]]]",

返回一个 NestedInteger 对象包含一个有两个元素的嵌套列表：

1. 一个 integer 包含值 123
2. 一个包含两个元素的嵌套列表：
	i.  一个 integer 包含值 456
	ii. 一个包含一个元素的嵌套列表
		a. 一个 integer 包含值 789
```

解法：普通不过的循环函数

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
class Solution {
    public int i = 0;

    public NestedInteger deserialize(String s) {
        char[] c = s.toCharArray();
        return process(c);
    }

    public NestedInteger process(char[] c) {
        if(c[i] == '[') {
            NestedInteger current = new NestedInteger();
            i++;
            while(c[i] != ']') {
                NestedInteger inside = process(c);
                current.add(inside);
            }
            i++;
            if(i < c.length && c[i] == ',')
                i++;
            return current;
        }
        else {
            boolean neg = false;
            if(c[i] == '-') {
                neg = true;
                i++;
            }
            int num = 0;
            while(i < c.length && Character.isDigit(c[i])) {
                num *= 10;
                num += c[i] - '0';
                i++;
            }
            if(neg)
                num = -num;
            if(i < c.length && c[i] == ',')
                i++;
            NestedInteger inside = new NestedInteger(num);
            return inside;
        }
    }
}
```

#### [423. 从英文中重建数字](https://leetcode-cn.com/problems/reconstruct-original-digits-from-english/)

给定一个非空字符串，其中包含字母顺序打乱的英文单词表示的数字0-9。按升序输出原始的数字。

注意:

输入只包含小写英文字母。
输入保证合法并可以转换为原始的数字，这意味着像 "abc" 或 "zerone" 的输入是不允许的。
输入字符串的长度小于 50,000。
示例 1:

```
输入: "owoztneoer"
输出: "012" (zeroonetwo)
```

示例 2:

```
输入: "fviefuro"
输出: "45" (fourfive)
```

解法：挺有趣的一题，纯数学分析之后计算即可，没有太多算法上的技巧

```java
public String originalDigits(String s) {

    Map<Character, Integer> charMap = new HashMap<>();
    for (char c : s.toCharArray()) {
        charMap.merge(c, 1, Integer::sum);
    }

    int[] out = new int[10];
    out[0] = getChar(charMap, 'z');
    out[2] = getChar(charMap, 'w');
    out[4] = getChar(charMap, 'u');
    out[6] = getChar(charMap, 'x');
    out[8] = getChar(charMap, 'g');
    out[3] = getChar(charMap, 'h') - out[8];
    out[5] = getChar(charMap, 'f') - out[4];
    out[7] = getChar(charMap, 's') - out[6];
    out[9] = getChar(charMap, 'i') - out[5] - out[6] - out[8];
    out[1] = getChar(charMap, 'n') - out[7] - 2 * out[9];

    // building output string
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < out[i]; j++) {
            sb.append(i);
        }
    }
    return sb.toString();
}

private int getChar(Map<Character, Integer> charMap, char z) {
    return charMap.getOrDefault(z, 0);
}
```

#### [424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

给你一个仅由大写英文字母组成的字符串，你可以将任意位置上的字符替换成另外的字符，总共可最多替换 k 次。在执行上述操作后，找到包含重复字母的最长子串的长度。

注意:
字符串长度 和 k 不会超过 104。

示例 1:

```
输入:
s = "ABAB", k = 2

输出:
4

解释:
用两个'A'替换为两个'B',反之亦然。
```


示例 2:

```
输入:
s = "AABABBA", k = 1

输出:
4

解释:
将中间的一个'A'替换为'B',字符串变为 "AABBBBA"。
子串 "BBBB" 有最长重复字母, 答案为 4。
```

<!--more-->

解法：维护一个滑动窗口，右边界逐步扩张，当且仅当窗口宽度大于【当前窗口最大重复字母长度+k】的时候，左边界收缩，一直滑动到最右，宽度即答案

![滑动窗口求解最长连续子串长度](https://i.loli.net/2020/07/12/oz1pAwe83krgMYf.png)

```java
public int characterReplacement(String s, int k) {
    int[] map = new int[26];
    if (s == null) {
        return 0;
    }
    char[] chars = s.toCharArray();
    int left = 0;
    int right = 0;
    int charMax = 0;
    for (right = 0; right < chars.length; right++) {
        int index = chars[right] - 'A';
        map[index]++;
        charMax = Math.max(charMax, map[index]);
        if (right - left + 1 > charMax + k) {
            map[chars[left] - 'A']--;
            left++;
        }
    }
    return right - left;
}
```

#### [1638. 统计只差一个字符的子串数目](https://leetcode-cn.com/problems/count-substrings-that-differ-by-one-character/)

给你两个字符串 s 和 t ，请你找出 s 中的非空子串的数目，这些子串满足替换 一个不同字符 以后，是 t 串的子串。换言之，请你找到 s 和 t 串中 恰好 只有一个字符不同的子字符串对的数目。

比方说， "**compute**r" 和 "**computa**tion" 加粗部分只有一个字符不同： 'e'/'a' ，所以这一对子字符串会给答案加 1 。

请你返回满足上述条件的不同子字符串对数目。

一个 子字符串 是一个字符串中连续的字符。

示例 1：

```
输入：s = "aba", t = "baba"
输出：6
```


示例 2：

```
输入：s = "ab", t = "bb"
输出：3
```

示例 3：

```
输入：s = "a", t = "a"
输出：0
```

示例 4：

```
输入：s = "abe", t = "bbc"
输出：10
```

提示：

```
1 <= s.length, t.length <= 100
s 和 t 都只包含小写英文字母。
```

解法：

```java
class Solution {

    public int countSubstrings(String s, String t) {
        int ans = 0;
        for (int i = 0; i < s.length(); i++) {
            ans += count(i, 0, s, t);
        }
        for (int i = 1; i < t.length(); i++) {
            ans += count(0, i, s, t);
        }
        return ans;
    }

    public int count(int i, int j, String s, String t) {
        int count = 0;
        int fij = 0, gij = 0;
        for (; i < s.length() && j < t.length(); i++, j++) {
            if (s.charAt(i) == t.charAt(j)) {
                gij++;
            } else {
                fij = gij + 1;
                gij = 0;
            }
            count += fij;
        }
        return count;
    }
}
```

