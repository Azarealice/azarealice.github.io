---
categories: 技术 - 算法
tags: 技术
date: 2021/1/1 14:30:00
---

### LeetCode - 代码模版

#### 求组合数C(m,n)

```java
public int comb(int m, int n) {
    m = Math.min(m, n - m);
    long ans = 1;
    for (int x = n - m + 1, y = 1; y <= m; ++x, ++y) {
        ans = ans * x / y;
    }
    return (int) ans;
}
```



#### 求最小公约数

```java
// x >= y
public int gcd(int x, int y) {
    return y == 0 ? x : gcd(y, x % y);
}
```

<!--more-->

