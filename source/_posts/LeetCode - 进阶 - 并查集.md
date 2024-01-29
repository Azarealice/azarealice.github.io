---
categories: 技术 - 算法
tags: 技术
date: 2021/1/2 16:00:00
---

### LeetCode - 进阶 - 并查集

#### [261. 以图判树](https://leetcode-cn.com/problems/graph-valid-tree/)

给定从 0 到 n-1 标号的 n 个结点，和一个无向边列表（每条边以结点对来表示），请编写一个函数用来判断这些边是否能够形成一个合法有效的树结构。

示例 1：

```
输入: n = 5, 边列表 edges = [[0,1], [0,2], [0,3], [1,4]]
输出: true
```


示例 2:

```
输入: n = 5, 边列表 edges = [[0,1], [1,2], [2,3], [1,3], [1,4]]
输出: false
```

注意：你可以假定边列表 edges 中不会出现重复的边。由于所有的边是无向边，边 [0,1] 和边 [1,0] 是相同的，因此不会同时出现在边列表 edges 中。

<!--more-->

```java
int[] root;

public boolean validTree(int n, int[][] edges) {
    if (n == 1 && edges.length == 0) {
        return true;
    }
    if (n < 1 || edges == null || edges.length != n - 1) {
        return false;
    }

    root = new int[n];
    for (int i = 0; i < n; i++) {
        root[i] = i;
    }
    for (int[] pair : edges) {
        int x = find(pair[0]);
        int y = find(pair[1]);
        if (x == y) {
            return false;
        }
        root[x] = y;
    }
    return true;

}

private int find(int i) {
    if (i != root[i]) {
        root[i] = find(root[i]);
    }
    return root[i];
}
```

#### [305. 岛屿数量 II](https://leetcode-cn.com/problems/number-of-islands-ii/)

假设你设计一个游戏，用一个 m 行 n 列的 2D 网格来存储你的游戏地图。

起始的时候，每个格子的地形都被默认标记为「水」。我们可以通过使用 addLand 进行操作，将位置 (row, col) 的「水」变成「陆地」。

你将会被给定一个列表，来记录所有需要被操作的位置，然后你需要返回计算出来 每次 addLand 操作后岛屿的数量。

注意：一个岛的定义是被「水」包围的「陆地」，通过水平方向或者垂直方向上相邻的陆地连接而成。你可以假设地图网格的四边均被无边无际的「水」所包围。

请仔细阅读下方示例与解析，更加深入了解岛屿的判定。

示例:

```
输入: m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]]
输出: [1,1,2,3]
```


解析:

起初，二维网格 grid 被全部注入「水」。（0 代表「水」，1 代表「陆地」）

```
0 0 0
0 0 0
0 0 0
```


操作 #1：addLand(0, 0) 将 grid\[0][0] 的水变为陆地。

```
1 0 0
0 0 0   岛屿的数量为 1
0 0 0
```


操作 #2：addLand(0, 1) 将 grid\[0][1] 的水变为陆地。

```
1 1 0
0 0 0   岛屿的数量为 1
0 0 0
```


操作 #3：addLand(1, 2) 将 grid\[1][2] 的水变为陆地。

```
1 1 0
0 0 1   岛屿的数量为 2
0 0 0
```


操作 #4：addLand(2, 1) 将 grid\[2][1] 的水变为陆地。

```
1 1 0
0 0 1   岛屿的数量为 3
0 1 0
```


拓展：

你是否能在 O(k log mn) 的时间复杂度程度内完成每次的计算？（k 表示 positions 的长度）

```java
public List<Integer> numIslands2(int m, int n, int[][] positions) {
    DisjointUnionSets disjointUnionSets = new DisjointUnionSets(m * n);
    List<Integer> ret = new ArrayList<>();
    for (int[] pos : positions) {
        int i = pos[0];
        int j = pos[1];
        int x = i * n + j;
        if (disjointUnionSets.insert(x) >= 0) {
            if (i - 1 >= 0 && disjointUnionSets.find(x - n) >= 0) {
                disjointUnionSets.union(x, x - n);
            }
            if (i + 1 < m && disjointUnionSets.find(x + n) >= 0) {
                disjointUnionSets.union(x, x + n);
            }
            if (j - 1 >= 0 && disjointUnionSets.find(x - 1) >= 0) {
                disjointUnionSets.union(x, x - 1);
            }
            if (j + 1 < n && disjointUnionSets.find(x + 1) >= 0) {
                disjointUnionSets.union(x, x + 1);
            }
        }
        ret.add(disjointUnionSets.c);
    }

    return ret;
}

class DisjointUnionSets {

    int[] rank, parent;
    int n;
    int c;

    public DisjointUnionSets(int n) {
        rank = new int[n];
        parent = new int[n];
        this.n = n;
        this.c = 0;
        makeSet();
    }

    void makeSet() {
        for (int i = 0; i < n; i++) {
            parent[i] = -1;
        }
    }

    int insert(int x) {
        if (parent[x] == -1) {
            parent[x] = x;
            c++;
            return x;
        } else {
            return -1;
        }
    }

    int find(int x) {
        if (parent[x] == -1) {
            return -1;
        }
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    void union(int x, int y) {
        int xRoot = find(x);
        int yRoot = find(y);
        if (xRoot == yRoot) {
            return;
        }

        if (rank[xRoot] < rank[yRoot]) {
            parent[xRoot] = yRoot;
        } else if (rank[yRoot] < rank[xRoot]) {
            parent[yRoot] = xRoot;
        } else {
            parent[yRoot] = xRoot;
            rank[xRoot] = rank[xRoot] + 1;
        }
        c--;
    }
}
```