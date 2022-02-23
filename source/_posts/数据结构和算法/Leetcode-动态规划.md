---
title: Leetcode-动态规划
date: 2022-02-23 13:51:44
tags: Leetcode-动态规划
description:
categories: Leetcode
---

## [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

难度简单

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：

输入： 2 输出： 2 解释： 有两种方法可以爬到楼顶。 1.  1 阶 + 1 阶 2.  2 阶

示例 2：

输入： 3 输出： 3 解释： 有三种方法可以爬到楼顶。 1.  1 阶 + 1 阶 + 1 阶 2.  1 阶 + 2 阶 3.  2 阶 + 1 阶

---

题解：第n阶的策略可以拆分成n-1阶走一步和n-2阶走两步

即f(n) =f(n-1)+f(n-2)

n= 1  1

n=2   2

n=3   3

n=4   5

```java
public static int climbStairs(int n) {
    int result = 0;
    int n1 = 1;
    int n2 = 2;
    if (n <= 2) {
        return n;
    } else {
        for (int i = 3; i <= n; i++) {
            result = n2 + n1;
            n1 = n2;
            n2 = result;
        }
    }
    return result;
}
```



动态规划：

注意**n+2；** 

```java
public static int climbStairs2(int n) {
        int[] dp = new int[n+2];
        dp[1] = 1;
        //当n=1时，int[n+2]长度才满足
        dp[2] = 2;
        for (int i = 3;i <= n; i++){
            dp[i] = dp[i-1] +dp[i-2];
        }
        return dp[n];
}

```

