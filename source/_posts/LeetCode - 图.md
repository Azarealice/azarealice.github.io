---
categories: 技术 - 算法
tags: 技术
date: 2021/1/1 21:30:00
---

### LeetCode - 图

#### [277. 搜寻名人](https://leetcode-cn.com/problems/find-the-celebrity/)

假设你是一个专业的狗仔，参加了一个 n 人派对，其中每个人被从 0 到 n - 1 标号。在这个派对人群当中可能存在一位 “名人”。所谓 “名人” 的定义是：其他所有 n - 1 个人都认识他/她，而他/她并不认识其他任何人。

现在你想要确认这个 “名人” 是谁，或者确定这里没有 “名人”。而你唯一能做的就是问诸如 “A 你好呀，请问你认不认识 B呀？” 的问题，以确定 A 是否认识 B。你需要在（渐近意义上）尽可能少的问题内来确定这位 “名人” 是谁（或者确定这里没有 “名人”）。

在本题中，你可以使用辅助函数 bool knows(a, b) 获取到 A 是否认识 B。请你来实现一个函数 int findCelebrity(n)。

派对最多只会有一个 “名人” 参加。若 “名人” 存在，请返回他/她的编号；若 “名人” 不存在，请返回 -1。

示例 1:

![img](https://i.loli.net/2021/01/01/7mZeLkhYpXfxoET.png)

```
输入: graph = [
  [1,1,0],
  [0,1,0],
  [1,1,1]
]
输出: 1
```

解释: 有编号分别为 0、1 和 2 的三个人。graph[i][j] = 1 代表编号为 i 的人认识编号为 j 的人，而 graph[i][j] = 0 则代表编号为 i 的人不认识编号为 j 的人。“名人” 是编号 1 的人，因为 0 和 2 均认识他/她，但 1 不认识任何人。

<!--more-->

示例 2:

![img](https://i.loli.net/2021/01/01/ZHUFNSY65agPv2T.png)

```
输入: graph = [
  [1,0,1],
  [1,1,0],
  [0,1,1]
]
输出: -1
```


解释: 没有 “名人”


提示：

```
n == graph.length
n == graph[i].length
2 <= n <= 100
graph[i][j] 是 0 或 1.
graph[i][i] == 1
```

解法：根据名人的特性，双指针逼近寻找候选人

```java
public int findCelebrity (int n) {
    int l = 0, r = n - 1;
    while (l != r) {
        if (knows(l, r)) l++;
        else r--;
    }

  for (int i = 0; i < n; i++) {
        if (i != l) {
            if (knows(l, i) || !knows(i, l)) return -1;
        }
    }
    return l;
}
```

#### [1192. 查找集群内的「关键连接」](https://leetcode-cn.com/problems/critical-connections-in-a-network/)

力扣数据中心有 n 台服务器，分别按从 0 到 n-1 的方式进行了编号。

它们之间以「服务器到服务器」点对点的形式相互连接组成了一个内部集群，其中连接 connections 是无向的。

从形式上讲，connections[i] = [a, b] 表示服务器 a 和 b 之间形成连接。任何服务器都可以直接或者间接地通过网络到达任何其他服务器。

「关键连接」是在该集群中的重要连接，也就是说，假如我们将它移除，便会导致某些服务器无法访问其他服务器。

请你以任意顺序返回该集群内的所有 「关键连接」。

 

示例 1：

![img](https://i.loli.net/2021/01/12/QuZr9TByEAzgDCj.png)
输入

```
输入：n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]
输出：[[1,3]]
解释：[[3,1]] 也是正确的。
```


提示：

```
1 <= n <= 10^5
n-1 <= connections.length <= 10^5
connections[i][0] != connections[i][1]
不存在重复的连接
```

解法：tarjan算法

```java
class Solution {

    private List<Integer>[] edges;
    private int[] DFN;
    private int[] LOW;
    private boolean[] visited;
    private List<List<Integer>> ans;
    private int t;

    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
        this.edges = new ArrayList[n];
        this.DFN = new int[n];
        this.LOW = new int[n];
        this.visited = new boolean[n];
        this.ans = new ArrayList<>();
        this.t = 0;
        for (int i = 0; i < n; i++) {
            edges[i] = new ArrayList<>();
        }
        for (List<Integer> conn : connections) {
            int n1 = conn.get(0), n2 = conn.get(1);
            edges[n1].add(n2);
            edges[n2].add(n1);
        }
        tarjan(0, -1);
        return ans;
    }

    public void tarjan(int cur, int pre) {
        t++;
        DFN[cur] = t;
        LOW[cur] = t;
        visited[cur] = true;
        for (int node : edges[cur]) {
            if (node == pre) {
                continue;
            }
            if (!visited[node]) {
                tarjan(node, cur);
                LOW[cur] = Math.min(LOW[cur], LOW[node]);
                if (LOW[node] > DFN[cur]) {
                    List<Integer> list = new ArrayList<>();
                    list.add(cur);
                    list.add(node);
                    ans.add(list);
                }
            } else {
                LOW[cur] = Math.min(LOW[cur], DFN[node]);
            }
        }
    }
}
```

