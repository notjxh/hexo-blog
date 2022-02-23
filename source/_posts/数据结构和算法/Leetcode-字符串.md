---
title: Leetcode-字符串
date: 2022-02-23 16:25:41
tags: Leetcode-字符串
description:
categories: Leetcode
---
## [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

难度中等

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

 

**示例 1:**

```text
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```text
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```text
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

 

**提示：**

- `0 <= s.length <= 5 * 104`
- `s` 由英文字母、数字、符号和空格组成



---

思路：遍历，分别找出以每一个字符开头的无重复字符的最长子串，取最大值



复杂度：

时间  n^2

遍历那个字符n *

分别找出最长子串 n

空间 n?

```java
/**
 * @param s
 * @return 3. 无重复字符的最长子串
 */
public static int lengthOfLongestSubstring(String s) {

    int maxLength = 0;
    char[] chars = s.toCharArray();

    //遍历所有字符
    for(int i = 0;i<s.length();i++){
        //以每个字符开头的最长
        int length = 0;
        Map map = new HashMap();
        for (int j = i; j < s.length(); j++) {
            if (!map.containsKey(chars[j])){
                map.put(chars[j],j);
                length++;
            }else {
                break;
            }
        }
        maxLength = maxLength>length?maxLength:length;

    }
    return maxLength;
}
```

执行用时：101 ms, 在所有 Java 提交中击败了7.63%的用户
内存消耗：39 MB, 在所有 Java 提交中击败了12.91%的用户





优化：不知道，看题解：滑动窗口

双指针分别指向最长子列的头和尾，依次递增枚举子串的头，那么尾也是递增的，原因是

假设以第k个元素为头，找出不重复的最长子串的尾部元素是rk，则

当以第k+1个元素为头，则从第k+1个元素到rk显然是不重复的，而且

由于已经除去了第k个元素，则可以尝试rk的后一个元素是否重复。

类似于滑动窗口



##  [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

难度中等

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

 

**示例 1：**

```text
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

**示例 2：**

```text
输入：s = "cbbd"
输出："bb"
```

 

**提示：**

- `1 <= s.length <= 1000`
- `s` 仅由数字和英文字母组成

---

思路：获取最长→遍历获取以第1-n位开头的最长palindrome

以第1位开头：怎么获取longest palindrome？

暴力破解：**116 / 180** 个通过测试用例 超时了



```java
 public static String longestPalindrome(String s) {
        int longest = 0;
        String longestPalindrome ="";

        //1,获取所有子串
        //2.判断是否是palindrome，记录长度和子串值
        //3.取出最长的
        int length = s.length();
        for (int i = 0; i < length; i++) {
            for (int j = i; j <=length; j++) {
                String subString = s.substring(i,j);
//                System.out.println(subString);
                if(isPalindrome(subString)&&subString.length()>longest){
                    longest = subString.length();
                    longestPalindrome = subString;
                }
            }
        }
        return longestPalindrome;

    }
    private static boolean isPalindrome(String s){
        for (int i = 0; i < s.length()/2; i++) {
            //注意这里的/2 和 -1
            if (s.charAt(i)!=s.charAt(s.length()-i-1)){
                return false;
            }
        }
        return true;
    }
```



中心遍历法

```java

public static String longestPalindrome2(String s) {
    int left,right;
    int longestPalindromeLength = 0;
    String longestPalindrome = "";
    for (int i = 0; i < s.length(); i++) {
        for (int j = 0; j <= 1; j++) {
            int maxLength = 0;
            left = i;
            right = i+j;
            while (left>=0&&right<s.length()&&s.charAt(left)==s.charAt(right)){
                maxLength = right - left +1;
                left--;
                right++;
            }
            if (maxLength>longestPalindromeLength){
                longestPalindromeLength = maxLength;
                longestPalindrome = s.substring(left+1,right);
            }
        }
    }
    return longestPalindrome;
}
```

执行用时：30 ms, 在所有 Java 提交中击败了68.89%的用户

内存消耗：38.5 MB, 在所有 Java 提交中击败了72.80%的用户





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