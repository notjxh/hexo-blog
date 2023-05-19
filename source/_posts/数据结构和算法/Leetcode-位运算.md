---
title: Leetcode-位运算
date: 2022-02-23 11:13:28
tags: Leetcode-位运算
description:
categories: Leetcode
---






## [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

难度简单

```text
两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 x 和 y，计算它们之间的汉明距离。

注意：

0 ≤ x, y < 231.

示例:

输入: x = 1, y = 4  输出: 2  解释: 1   (0 0 0 1) 4   (0 1 0 0)        ↑   ↑  上面的箭头指出了对应二进制位不同的位置。

```



思路;用异或计算，输出不为0的二进制个数

```java
/**
 * @param x
 * @param y
 * @return 461. 汉明距离
 */
public int hammingDistance(int x, int y) {

    Integer result = x ^ y;
    String s = Integer.toBinaryString(result);
    int count = 0;
    for (char c : s.toCharArray()) {
        if (c!='0'){
            count++;
        }
    }
    return count;
}
```



缺点：效率太低



执行用时：1 ms, 在所有 Java 提交中击败了9.62%的用户

内存消耗：35.2 MB, 在所有 Java 提交中击败了65.91%的用户



用以下内置**位计数功能**超过100%了~

return Integer.bitCount(x ^ y); 



其他解法 **布赖恩·克尼根算法 详见官方题解**



## [338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)

难度简单

给你一个整数 `n` ，对于 `0 <= i <= n` 中的每个 `i` ，计算其二进制表示中 **1 的个数** ，返回一个长度为 `n + 1` 的数组 `ans` 作为答案。

 

**示例 1：**

```text
输入：n = 2
输出：[0,1,1]
解释：
0 --> 0
1 --> 1
2 --> 10
```

**示例 2：**

```text
输入：n = 5
输出：[0,1,1,2,1,2]
解释：
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101
```

 

**提示：**

- `0 <= n <= 105`

 

**进阶：**

- 很容易就能实现时间复杂度为 `O(n log n)` 的解决方案，你可以在线性时间复杂度 `O(n)` 内用一趟扫描解决此问题吗？
- 你能不使用任何内置函数解决此问题吗？（如，C++ 中的 `__builtin_popcount` ）

---

思路：无，看题解，有个思路如下：

> 所有的数只能是偶数或者奇数

偶数x：最后一位一定是0，含1量==x/2的；相当于x`右移`一位等到x/2

如 1（1） 2（10）  4（100） 3（11) 6(110)

奇数y：相当于前一位偶数的最后一位变成了1，含1量==前一个奇数的+1

如 1（1） 2（10）  3（11）  4（100）5（101）

故可以通过动态规划来解决：

```
class Solution {
    public int[] countBits(int n) {
        int[] result = new int[n+1];
        result[0]=0;
        for (int i = 1;i<=n;i++){
            //奇数
            if (i%2!=0){
                result[i]= result[i-1]+1;
            }else {
                result[i]= result[i/2];
            }
        }
        return result;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了99.97%的用户

内存消耗：45.5 MB, 在所有 Java 提交中击败了44.92%的用户