---
categories: 技术 - 算法
tags: 技术
date: 2020/7/25 16:20:00
---

### LeetCode - 链表

#### [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

示例：

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```


说明：

给定的 n 保证是有效的。

你能尝试使用一趟扫描实现吗？

<!--more-->

解法：涉及到一趟扫描&倒数处理的前提，必然想到使用双指针，这里让快指针先走n步，慢指针再出发，当快指针到达末尾的时候慢指针就会到达倒数n个节点的前一个节点。

```java
public ListNode removeNthFromEnd(ListNode head, int n) {

    ListNode fast = head;
    ListNode slow = head;
    ListNode prev = null;

    while (n > 1) {
        fast = fast.next;
        n--;
    }

    while (fast.next != null) {
        fast = fast.next;
        prev = slow;
        slow = slow.next;
    }

    if (prev != null) {
        prev.next = slow.next;
        return head;
    } else {
        return slow == null ? null : slow.next;
    }
}
```

#### [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

示例:

```
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```

解法：当涉及多个数字进行比较，并且每次比较出一进一的情况，需要考虑使用PriorityQueue（堆排序），这里把k个链表表头均加入PriorityQueue，每次取出最小的元素并放入该元素的尾节点（如果存在），复杂度为O(kn * log(k))，n为链表最大长度
```java
public ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> priorityQueue = new PriorityQueue(new Comparator<ListNode>() {
        @Override
        public int compare(ListNode l1, ListNode l2) {
            return l1.val - l2.val;
        }
        @Override
        public boolean equals(Object obj) {
            return false;
        }
    });

    int k = lists.length;

    for (int i = 0; i < k; i++) {
        if (lists[i] != null) {
            priorityQueue.offer(lists[i]);
        }
    }

    ListNode preHead = new ListNode(-1);
    ListNode end = preHead;

    while (!priorityQueue.isEmpty()) {
        ListNode poll = priorityQueue.poll();
        end.next = new ListNode(poll.val);
        end = end.next;

        if (poll.next != null) {
            priorityQueue.offer(poll.next);
        }
    }

    return preHead.next;
}
```

#### [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

示例：

```
给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5 
```

说明：

你的算法只能使用常数的额外空间。
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

解法：这题被标记为困难难度，其实就是逻辑上比较复杂而已，理清节点互挂关系之后就水到渠成，不可急躁。此处维护四个额外节点，虚拟的节点头prev，新链表尾部prevEnd，需要翻转的头部节点rHead，目前的节点current，本执行用时在所有 Java 提交中击败了100.00%的用户。

```java
public ListNode reverseKGroup(ListNode head, int k) {
    if (k == 1) {
        return head;
    }
    ListNode prev = new ListNode(-1);
    ListNode prevEnd = prev;

    prev.next = head;
    ListNode rHead = head;
    ListNode current = prev;
    int x = k;
    while (current != null) {
        current = current.next;
        x--;
        if (x == 0 && current != null) {
            ListNode next = current.next;
            current.next = null;
            prevEnd.next = reverseListNode(rHead);
            x = k;
            current = rHead;
            prevEnd = rHead;
            rHead = next;
            current.next = next;
        }
    }

    return prev.next;

}

private ListNode reverseListNode(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode p = reverseListNode(head.next);
    head.next.next = head;
    head.next = null;
    return p;
}
```

#### [138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。

要求返回这个链表的 深拷贝。 

我们用一个由 n 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 [val, random_index] 表示：

val：一个表示 Node.val 的整数。
random_index：随机指针指向的节点索引（范围从 0 到 n-1）；如果不指向任何节点，则为  null 。


示例 1：

![img](https://i.loli.net/2020/07/26/BmMt6b4AF1ElOIs.png)

输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]

解法一：直接深度遍历复制，代码简洁，缺点是需要额外空间O(n)用于维护哈希表

```java
HashMap<Node, Node> visitedHash = new HashMap<Node, Node>();

public Node copyRandomList(Node head) {
    if (head == null) {
        return null;
    }
    if (this.visitedHash.containsKey(head)) {
        return this.visitedHash.get(head);
    }
    Node node = new Node(head.val);
    this.visitedHash.put(head, node);
    node.next = this.copyRandomList(head.next);
    node.random = this.copyRandomList(head.random);

    return node;
}
```

解法二：如果想要不使用O(n)额外空间，那随机指针的字典如何维护？那我们唯一能够利用的只剩下入参，即原链表。此处将旧链表所有节点原地复制val和next，然后挂到各自的原节点后面，即原链表将原地膨胀一倍。膨胀结束之后，再次遍历链表，使复制节点的random值等于原节点random节点的next节点的即可。如图A'的random想要指向C'，其实就是A.next.random=A.random.next。最后将原链表和复制链表拆开即可。
![image.png](https://i.loli.net/2020/07/26/BCtV3QovkA4wyN2.png)

```java
public Node copyRandomList(Node head) {
    if (head == null) {
        return null;
    }
    Node current = head;

    while (current != null) {
        Node copy = new Node(current.val);
        Node tmp = current.next;
        copy.next = tmp;
        current.next = copy;

        current = tmp;
    }

    current = head;
    while (current != null) {
        if (current.random != null) {
            current.next.random = current.random.next;
        }
        current = current.next.next;
    }

    Node copyHead = new Node(-1);
    Node copyEnd = copyHead;
    current = head;
    while (current != null) {
        copyEnd.next = current.next;
        copyEnd = copyEnd.next;

        current.next = current.next.next;
        current = current.next;
    }

    return copyHead.next;
}
```

#### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表。

 

示例 1：

```
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
```

<img src="https://i.loli.net/2020/07/26/saM324itvIYbnkL.png" alt="img" style="zoom:50%;" />

示例 2：

```
输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。
```

<img src="https://i.loli.net/2020/07/26/PKj71NOsRkwT8l2.png" alt="img" style="zoom:50%;" />

 示例 3：

```
输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。
```

<img src="https://i.loli.net/2020/07/26/C3DBuRTqnYZM6GH.png" alt="img" style="zoom:50%;" />

进阶：
你是否可以不用额外空间解决此题？

解法：可用哈希表解决此问题，但有更为巧妙的纯数学floyd法，简述：快指针走过的路径l，慢指针走过的路径s，相遇时可知l=2s，同时快指针比慢指针多跑了n圈，设圈长度为b，l=s+nb，可得s=nb，慢指针走过的路径竟然恰好等于绕圈走了n圈的长度。设入口到环形入口为a，可知任何一个走过了a+nb的指针都会位于环形入口，故此处只需要让慢指针再走a步即可，新建一个指针于表头，和慢指针一起出发，两者相遇时即为环形入口。

```java
public ListNode detectCycle(ListNode head) {
    if (head == null || head.next == null) {
        return null;
    }

    ListNode slow = head.next;
    ListNode fast = head.next.next;

    Boolean flag = false;
    while (fast != null) {
        if (fast == slow) {
            flag = true;
            break;
        }

        if (fast.next == null) {
            return null;
        }

        fast = fast.next.next;
        slow = slow.next;
    }

    if (flag) {
        ListNode x = head;
        while (x != slow) {
            x = x.next;
            slow = slow.next;
        }
        return slow;
    } else {
        return null;
    }
}
```
#### [147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

对链表进行插入排序。

插入排序算法：

插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
重复直到所有输入数据插入完为止。


示例 1：

```
输入: 4->2->1->3
输出: 1->2->3->4
```


示例 2：

```
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```

解法：没有特别复杂的地方，但能写好也是重要的基础


```java
public ListNode insertionSortList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode dummy = new ListNode(-1);
    dummy.next = head;

    ListNode pre = head;
    ListNode current = head.next;

    while (current != null) {
        if (pre.val <= current.val) {
            pre = current;
            current = current.next;
        } else {
            ListNode p = dummy;
            while (p.next.val < current.val) {
                p = p.next;
            }
            pre.next = current.next;
            current.next = p.next;
            p.next = current;
            current = pre.next;
        }
    }

    return dummy.next;
}
```

#### [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序。

示例 1:

```
输入: 4->2->1->3
输出: 1->2->3->4
```


示例 2:

```
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```

解法：在排序性能能达到O(n log n)的算法中，最常见的三种，快速排序/归并排序/堆排序中，此处显然考虑使用归并排序来排序链表最为合理，因为归并过程对链表的结构非常友好，其他两种都会涉及频繁的指针指向操作。还是那句话，把简单的东西写好也是一种重要技能。

```java
public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }

    ListNode slow = head;
    ListNode fast = head.next.next;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    ListNode head2 = slow.next;
    slow.next = null;

    return merge(sortList(head), sortList(head2));
}

private ListNode merge(ListNode x, ListNode y) {
    ListNode preHead = new ListNode(-1);

    ListNode current = preHead;
    while (x != null && y != null) {
        if (x.val < y.val) {
            current.next = x;
            x = x.next;
            current = current.next;
            current.next = null;
        } else {
            current.next = y;
            y = y.next;
            current = current.next;
            current.next = null;
        }
    }
    if (x != null) {
        current.next = x;
    }
    if (y != null) {
        current.next = y;
    }

    return preHead.next;
}
```

#### [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

编写一个程序，找到两个单链表相交的起始节点。

示例 1：
<img src="https://i.loli.net/2020/07/26/wMh4YELt7OQGfn5.png" alt="img" style="zoom:50%;" />

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

注意：

如果两个链表没有交点，返回 null.
在返回结果后，两个链表仍须保持原有的结构。
可假定整个链表结构中没有循环。
程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。

解法：虽然标记为简单题但有非常精彩的解法，链表题当要求使用O(n)时间复杂度+O(1)空间复杂度时，应该想到双指针/指针移动，此处为指针移动，A指针走完指向B开头，反之亦然。如例图中，5+3=6+2，所以如果相交，两个指针终会相遇。

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) return null;
    ListNode pA = headA, pB = headB;
    while (pA != pB) {
        pA = pA == null ? headB : pA.next;
        pB = pB == null ? headA : pB.next;
    }
    return pA;
}
```

#### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

反转一个单链表。

示例:

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }

    ListNode p = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return p;
}

public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```

#### [1171. 从链表中删去总和值为零的连续节点](https://leetcode-cn.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/)

给你一个链表的头节点 head，请你编写代码，反复删去链表中由 总和 值为 0 的连续节点组成的序列，直到不存在这样的序列为止。

删除完毕后，请你返回最终结果链表的头节点。

 

你可以返回任何满足题目要求的答案。

（注意，下面示例中的所有序列，都是对 ListNode 对象序列化的表示。）

示例 1：

```
输入：head = [1,2,-3,3,1]
输出：[3,1]
提示：答案 [1,2,1] 也是正确的。
```


示例 2：

```
输入：head = [1,2,3,-3,4]
输出：[1,2,4]
```


示例 3：

```
输入：head = [1,2,3,-3,-2]
输出：[1]
```


提示：

给你的链表中可能有 1 到 1000 个节点。
对于链表中的每个节点，节点的值：-1000 <= node.val <= 1000.

解法：如果从i到j的元素和为0，代表[0,i-1]和[0,j]两个区间的区间和相等，基于这个思路将每个节点和从头节点到本节点到区间和存储于哈希表中，如果发现区间和相等的index，直接将当前节点的next挂在相等区间和末尾的next上，即i-1.next = j.next

```java
public ListNode removeZeroSumSublists(ListNode head) {

    ListNode dummy  = new ListNode(0);
    dummy.next = head;
    ListNode p = dummy;
    int sum = 0;
    Map<Integer, ListNode> map = new HashMap<>();

    while (p != null) {
        sum += p.val;
        map.put(sum, p);

        p = p.next;
    }

    p = dummy;
    sum = 0;
    while (p != null) {
        sum += p.val;
        if (map.get(sum) != p) {
            p.next = map.get(sum).next;
        }
        p = p.next;
    }

    return dummy.next;
}
```