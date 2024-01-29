---
categories: 技术 - 算法
tags: 技术
date: 2020/7/25 16:17:00
---

### LeetCode - 数组

#### [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

 

示例 1：

```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```


提示：

1 <= values <= 10000
最多会对 appendTail、deleteHead 进行 10000 次调用

<!--more-->

解法：

```java
class CQueue {

    private Stack<Integer> in;
    private Stack<Integer> out;

    public CQueue() {
        in = new Stack<>();
        out = new Stack<>();
    }

    public void appendTail(int value) {
        in.push(value);
    }

    public int deleteHead() {
        if (out.isEmpty()) {
            if (in.isEmpty()) {
                return -1;
            } else {
                while (!in.isEmpty()) {
                    out.push(in.pop());
                }
            }
        }
        return out.pop();
    }
}
```





