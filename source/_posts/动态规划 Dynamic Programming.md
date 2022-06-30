---
title: 动态规划 Dynamic Programming
urlname: zaxcbe
date: 2020-01-12 00:00:00 +0800
tags: [算法]
categories: [算法]
---

##### 参考

- [Wiki 上定义](https://en.wikipedia.org/wiki/Dynamic_programming)
- [mit 动态规划](https://www.bilibili.com/video/av53233912?from=search&seid=2847395688604491997)

<!-- more -->

##### 动态规划关键点

- 动态规划 和 递归或者分治 没有根本上的区别(关键看有无最优的子结构)
- 共性:找到重复子问题
- 差异性:最优子结构、中途可以淘汰次优解

##### 动态规划小结

- 打破自己的思维惯性，形成机器思维
- 理解复杂逻辑的关键
- 也是职业进阶的要点要领

#### 斐波拉契数列

```java
int fib (int n) {
    return n <= 1 ? n : fib (n - 1) + fib (n - 2);
}
```

#### 路径计数问题

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602937708682-ca2bd5a6-09af-481e-831f-fdc06eade4b0.png#align=left&display=inline&height=2328&margin=%5Bobject%20Object%5D&originHeight=2328&originWidth=3208&size=0&status=done&style=none&width=3208)

```javascript
 状态转移方程(DP 方程)
 opt[i , j] = opt[i + 1, j] + opt[i, j + 1] 完整逻辑:
 if a[i, j] = '空地':
 opt[i , j] = opt[i + 1, j] + opt[i, j + 1] else:
 opt[i , j] = 0
```

#### 问题关键点

- 最优子结构 opt[n] = best_of(opt[n-1], opt[n-2], ...)
- 储存中间状态:opt[i]
- 递推公式(美其名曰:状态转移方程或者 DP 方程)
  Fib: opt[i] = opt[n-1] + opt[n-2]
  二维路径:opt[i,j] = opt[i+1][j] + opt[i][j+1] (且判断 a[i,j]是否空地)
