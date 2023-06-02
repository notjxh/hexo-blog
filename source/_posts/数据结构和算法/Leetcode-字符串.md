---
title: Leetcode-字符串
date: 2022-02-23 16:25:41
tags: Leetcode-字符串
description:
categories: Leetcode
---



##  [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

难度中等

给你一个字符串 `s` ，请你统计并返回这个字符串中 **回文子串** 的数目。

**回文字符串** 是正着读和倒过来读一样的字符串。

**子字符串** 是字符串中的由连续字符组成的一个序列。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

 

**示例 1：**

```text
输入：s = "abc"
输出：3
解释：三个回文子串: "a", "b", "c"
```

**示例 2：**

```text
输入：s = "aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
```

 

**提示：**

- `1 <= s.length <= 1000`
- `s` 由小写英文字母组成

---

```java
 public int countSubstrings(String s) {
        //分中心位置为一个字符和两个字符的情况
        //分别遍历中心位置并且向两边扩展判断是否是回文串
        int length = s.length();
        int left,right;
        int sum = 0;
        for(int i= 0;i<length;i++){
            //j=0时，left，right是一个位置，表示奇数位数的回文串 j=1时表示偶数
            for (int j = 0;j<=1;j++){
                left=i;
                right=i+j;
                while(left>=0 && right<length && s.charAt(left)==s.charAt(right)){
                    sum++;
                    left--;
                    right++;
                }
            }
        }
        return sum;
    }
```

执行用时：3 ms, 在所有 Java 提交中击败了88.02%的用户

内存消耗：36.6 MB, 在所有 Java 提交中击败了63.82%的用户




## [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

难度简单534收藏分享切换为英文接收动态反馈

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `s` 的形式给出。

不要给另外的数组分配额外的空间，你必须**原地修改输入数组**、使用 O(1) 的额外空间解决这一问题。



**示例 1：**

```
输入：s = ["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

**示例 2：**

```
输入：s = ["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```



**提示：**

- `1 <= s.length <= 105`
- `s[i]` 都是 [ASCII](https://baike.baidu.com/item/ASCII) 码表中的可打印字符

---

方法一：

```java
    public void reverseString(char[] s) {
        int length = s.length;
        for(int i = 0;i<length/2;i++){
            if(i<length-i-1){
            char temp = s[i];
            s[i] = s[length-i-1];
            s[length-i-1] = temp;
            }
        }
        
    }
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：47.7 MB, 在所有 Java 提交中击败了35.76%的用户

方法二：

如何使用`递归`的思路解？

求s[0]..s[n]的反转，则先反转s[0]和s[n]；再递归反转剩下的s[1]...s[n-1]

四步走：

设置递归的结束条件，防止死循环：剩余s的长度<=2

递之前的操作：将首尾相交换

递归下去的操作：将剩下的继续递归

归之后的操作：无

```java
    public void reverseString(char[] s) {
        reverse(s,0,s.length-1);
    }
    public void reverse(char[] s,int left,int right){
        //设置结束条件
        if(right-left<=0){
            return;
        }
        //递归之前的操作：
        char temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        //递归下去
        reverse(s,left+1,right-1);
        //之后的操作：无
    }
```

执行用时：1 ms, 在所有 Java 提交中击败了35.54%的用户

内存消耗：49.2 MB, 在所有 Java 提交中击败了5.11%的用户



方法三：异或运算

```java
public void reverseString(char[] s) {
        int n = s.length;
        for (int i = 0; i < n / 2; ++i) {
            int j = n - 1 - i;
            s[i] ^= s[j];
            s[j] ^= s[i];
            s[i] ^= s[j];
        }
    }
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：47.8 MB, 在所有 Java 提交中击败了31.67%的用户