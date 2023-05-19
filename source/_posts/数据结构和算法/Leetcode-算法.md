---
title: Leetcode-数组
date: 2022-02-23 09:59:45
tags: Leetcode-数组
description:
categories: Leetcode
---
# 哈希

## [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

难度简单

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *target*  的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

 

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

 

**提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

 

**进阶：**你可以想出一个时间复杂度小于 `O(n2)` 的算法吗？

---

思路：
暴力：将从头将每个值与其后的所有值一个个相加，得到target返回
```java
public int[] twoSum(int[] nums, int target) {
       int length = nums.length;
       for (int i = 0; i < length; i++) {
           for (int j = i+1; j < length; j++) {
               if (nums[i]+nums[j]==target){
                   return new int[]{i,j};
               }
           }
       }
       return null;
   }
```

将时间复杂度从O(n²)降到O(n)：通过哈希
https://blog.csdn.net/weixin_44617285/article/details/105507811

```java
/**
 * @return 上一方法的时间复杂度是O(n²)，原因是第二次遍历查找时的复杂度也是O(n)
 * 考虑将第二次查找改为hash查找(复杂度O(1))
 */
public int[] twoSum2(int[] nums, int target) {
    HashMap<Integer,Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(target-nums[i])){
            return new int[]{i,map.get(target-nums[i])};
        }
        map.put(nums[i],i);
    }
    return null;
}
```



## [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/?envType=study-plan-v2&id=top-100-liked)

中等

给你一个字符串数组，请你将 **字母异位词** 组合在一起。可以按任意顺序返回结果列表。

**字母异位词** 是由重新排列源单词的字母得到的一个新单词，所有源单词中的字母通常恰好只用一次。

 

**示例 1:**

```
输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**示例 2:**

```
输入: strs = [""]
输出: [[""]]
```

**示例 3:**

```
输入: strs = ["a"]
输出: [["a"]]
```

 

**提示：**

- `1 <= strs.length <= 104`
- `0 <= strs[i].length <= 100`
- `strs[i]` 仅包含小写字母

```java
    /**
     * @param strs
     * @return 49. 字母异位词分组
     * 思路： 创建一个HashMap<String ,List<String>
     * 将每个strs排序后作为key，strs作为list的值存进去
     * 按照key 取出来转换成数组
     */
    public static List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String,List<String>> map = new HashMap<>();
        for (String str : strs) {
            char[] chars = str.toCharArray();
            Arrays.sort(chars);
            //注意：toString返回的事对象地址
//            String key = chars.toString();
            String key = new String(chars);
            List<String> strings = map.getOrDefault(key,new ArrayList<String>());
            strings.add(str);
            map.put(key,strings);
        }
        return new ArrayList<List<String>>(map.values());

    }
```



## [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/?envType=study-plan-v2&id=top-100-liked)

中等

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

**示例 2：**

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

 

**提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

```java
    /**
     * @param nums
     * @return 128. 最长连续序列
     * 思路:要求O(n)的时间复杂度，那么必须每个数字只处理一次
     * 对于最长的序列 n , n+1 , n+2 ...,数组中必然不存在n-1
     * 对于当前的数 n, 如果存在n-1 ，则跳过不处理，
     * 如果不存在n-1 ,则 从n+1 开始取到最大的，就是当前的最长子列
     * 9,1,4,7,3,-1,0,5,8,-1,6
     */
    public static int longestConsecutive(int[] nums) {
        int longest = 0;

        HashSet<Integer> numsSet = new HashSet<>();
        for (int num : nums) {
            numsSet.add(num);
        }

        for (int i : numsSet) {
            int currentNum = i;
            if (!numsSet.contains(currentNum - 1)) {
                int currentLength = 1;
                while (numsSet.contains(currentNum + 1)) {
                    currentLength++;
                    currentNum++;
                }
                longest = Math.max(longest, currentLength);
            }
        }
        return longest;
    }
```



# 双指针



## [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

难度简单

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:

输入: [0,1,0,3,12] 输出: [1,3,12,0,0]

说明:

1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。

------

思路：

定义两个指针，i指向当前元素，j指向非零元素 如果nums[i]!=0,将其移动到非零元素j的位置，然后补充后面的0

```java
public static void moveZeroes(int[] nums) {
    int j = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i]!=0){
            nums[j++]=nums[i];
        }
    }
    for (int i = j; i < nums.length; i++) {
        nums[i]=0;
    }

}
```



优化：只做一次循环：需要画图理清思路

如果nums[i]!=0,则j就指向下一项，直到找到0项为止，

找到后和之后的第一个非零项交换，再指向下一项，此时下一项一定是0

```java
public static void moveZeroes2(int[] nums) {
    int j = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i]!=0){
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] =temp;
            //j指向下一位，当再次循环i的时候，如果nums[i]==0，则j指向的就是找到的第一个0项位置，i继续增加，找到非零项，和j指向的第一个0项交换位置
            //j继续指向下一位，此时一定是0项位
            j++;
        }
    }
}
```





# 滑动窗口



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



------

思路：遍历，分别找出以每一个字符开头的无重复字符的最长子串长度，取最大值

```java
    public static int lengthOfLongestSubstring(String s) {

        int maxLength = 0;
        char[] chars = s.toCharArray();

        //遍历所有字符
        for (int i = 0; i < s.length(); i++) {
            //以每个字符开头的最长
            int length = 0;
            HashSet<Character> set = new HashSet<>();
            for (int j = i; j < s.length(); j++) {
                if (!set.contains(chars[j])) {
                    set.add(chars[j]);
                    length++;
                } else {
                    break;
                }
            }
            maxLength = maxLength > length ? maxLength : length;

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

```java
    public static int lengthOfLongestSubstring2(String s) {

        Set<Character> set = new HashSet<>();
        int maxLength = 0;
        int right = 0;
        for (int left = 0; left < s.length(); left++) {
            while (right < s.length() && !set.contains(s.charAt(right))) {
                set.add(s.charAt(right));
                right++;
            }
            maxLength = Math.max(maxLength, set.size());
            set.remove(s.charAt(left));
        }

        return maxLength;
    }
```



## [438. 找到字符串中所有字母异位词](/https://leetcode.cn/problems/find-all-anagrams-in-a-string/?envType=study-plan-v2&id=top-100-liked)

中等

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

**异位词** 指由相同字母重排列形成的字符串（包括相同的字符串）。

 

**示例 1:**

```
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```

 **示例 2:**

```
输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

 

**提示:**

- `1 <= s.length, p.length <= 3 * 104`
- `s` 和 `p` 仅包含小写字母

------

思路：依次遍历字符串s，判断以当前字符为开头的子串是否满足是p的异位词 

优化：那么下一个去掉头，增加尾就能判断是否是异位词了

首先需要怎么判断两个字符串s，p是异位词？通过数组的形式：

新建数组new int[26] sArr如果s中有个a ，则sArr[0]位置的数+1，以此类推。。

sArr[s-'a']++ 

然后通过Arrays.equals(sArr,pArr)判断是否相同

```java
    public static List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans = new ArrayList<>();
        int sl = s.length();
        int pl = p.length();
        if (sl < pl) {
            return ans;
        }

        //初始化两个数组
        int[] pArr = new int[26];
        int[] sArr = new int[26];
        for (int i = 0; i < pl; i++) {
            pArr[p.charAt(i) - 'a']++;
            sArr[s.charAt(i) - 'a']++;
        }
        if (Arrays.equals(pArr, sArr)) {
            ans.add(0);
        }

        for (int i = 0; i < sl - pl; i++) {
            sArr[s.charAt(i) - 'a']--;
            sArr[s.charAt(i + pl) - 'a']++;
            if (Arrays.equals(sArr, pArr)) {
                ans.add(i+1);
            }
        }
        return ans;

    }
```

优化todo：通过diff判断是否相等。。。



# 子串



## [560. 和为 K 的子数组](/https://leetcode.cn/problems/subarray-sum-equals-k/description/?envType=study-plan-v2&id=top-100-liked)



中等



给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 k 的连续子数组的个数* 。

 

**示例 1：**

```
输入：nums = [1,1,1], k = 2
输出：2
```

**示例 2：**

```
输入：nums = [1,2,3], k = 3
输出：2
```

 

**提示：**

- `1 <= nums.length <= 2 * 104`
- `-1000 <= nums[i] <= 1000`
- `-107 <= k <= 107`



```
    /**
     * @param nums 560. 和为 K 的子数组
     * @param k
     * @return 考虑以nums[i]开头的子数列，遍历所有nums[i]到num[j]满足和为k
     */
    public static int subarraySum(int[] nums, int k) {
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            int result = 0;
            for (int j = i; j < nums.length; j++) {
                result += nums[j];
                if (result == k) {
                    count++;
                }
            }
        }
        return count;

    }
```



```
        /**
     * @param nums 利用前缀法，降低复杂度到o(n)
     *             定义F(i)为从nums[0]到nums[i]的子序列和，那么
     *             对于从nums[i]-nums[j]的子序列，它的和f(ij)=F(j)-F(i-1),j>=i>=0;
     *             如果f(ij)=F(j)-F(i-1)=K。则满足条件
     *             依次计算F(i)的值并存储到map中，value为出现的次数，
     *             如果map中存在k-F(i)的值则count+1；
     * @param k
     * @return
     * {5, 1, 4, 7, 3, -1, 0, 5, 8, -1, 6};5
     * [1,-1,0];0==3
     *
     */
    public static int subarraySum2(int[] nums, int k) {
        int count = 0;
        int sum = 0;
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        for (int num : nums) {
            sum = sum + num;
            Integer orDefault = map.getOrDefault(sum, 0);
            if (map.containsKey(sum - k)) {
                count += map.get(sum-k);
            }
            map.put(sum, orDefault +1);
        }
        return count;

    }
```

总结:和53比较







# 普通数组



##  [53. 最大子序和todo](https://leetcode-cn.com/problems/maximum-subarray/)


给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例 1：

输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
示例 2：

输入：nums = [1]
输出：1
示例 3：

输入：nums = [0]
输出：0
示例 4：

输入：nums = [-1]
输出：-1
示例 5：

输入：nums = [-100000]
输出：-100000


 提示：

 1 <= nums.length <= 10^5
 -10^4 <= nums[i] <= 10^4

进阶：如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的 分治法 求解。



最大和连续子列一定是以某个节点为`结尾`的连续子列
我们遍历出以所有节点为结束点的连续子列，计算出他们的最大和，比较取出最大值
要求以第i个元素为结束点的子列的最大和，只需要比较以第i-1个元素的结束点的最大子列

```java
public int maxSubArray(int[] nums) {
    //preSum = 表示以当前节点为结束节点的连续子列的最大和，分为两个部分，
    //以前一个节点为结束节点的连续子列的最大和preSum，和 nums[i]
    //如果preSum<0,则显然取nums[i]
    int preSum = 0 ;
    //maxSum表示以所有节点为结束节点的最大和中的最大值
    int maxSum =nums[0];
    for (int num : nums) {
        preSum = Math.max(num,preSum+num);
        maxSum = Math.max(maxSum,preSum);
    }
    return maxSum;
}
```

还有一种 todo分治法：





# 贪心算法



## [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

中等



给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

示例 1：

输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
示例 2：

输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。

提示：

1 <= prices.length <= 105
0 <= prices[i] <= 104

------

思路：
第i天的最大利润=第i天的价格-前i-1天的最低价格
比较第i-1天的最大利润，取最大值

```java
    /**
     * @param prices
     * @return 121. 买卖股票的最佳时机
     * 第i天最高价格=Max(第n-1天的最高价格，price[i]-min(前n-1))
     */
    public static int maxProfit(int[] prices) {
        if (prices.length<2){
            return 0;
        }
        int maxProfit = 0;
        int minPrice = prices[0];
        for (int i = 1; i < prices.length; i++) {
            maxProfit = Math.max(prices[i]-minPrice,maxProfit);
            minPrice = Math.min(minPrice,prices[i]);
        }
        return maxProfit;
    }
```



## [55. 跳跃游戏](/https://leetcode.cn/problems/jump-game/description/)

中等

给定一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标。

 

**示例 1：**

```
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```

**示例 2：**

```
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```

 

**提示：**

- `1 <= nums.length <= 3 * 104`
- `0 <= nums[i] <= 105`

---



思路：看题解，贪心算法

```java
    /**
     * @param nums
     * @return 55. 跳跃游戏
     * 思路：依次计算当前点能到达的最大距离，判断是否能到达终点
     * 先判断是否能到达当前点
     * Max(i+nums[i])>=nums.length-1?
     *
     */
    public static boolean canJump(int[] nums) {
        int maxLength = 0;
        int length = nums.length;
        for (int i = 0; i < length; i++) {
            if (i<=maxLength){
                maxLength = Math.max(maxLength,i+nums[i]);
                if (maxLength>=length-1){
                    return true;
                }
            }
        }
        return false;
    }
```



## [45. 跳跃游戏 II](/https://leetcode.cn/problems/jump-game-ii/description/)

中等

给定一个长度为 `n` 的 **0 索引**整数数组 `nums`。初始位置为 `nums[0]`。

每个元素 `nums[i]` 表示从索引 `i` 向前跳转的最大长度。换句话说，如果你在 `nums[i]` 处，你可以跳转到任意 `nums[i + j]` 处:

- `0 <= j <= nums[i]` 
- `i + j < n`

返回到达 `nums[n - 1]` 的最小跳跃次数。生成的测试用例可以到达 `nums[n - 1]`。

 

**示例 1:**

```
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**示例 2:**

```
输入: nums = [2,3,0,1,4]
输出: 2
```

 

**提示:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`
- 题目保证可以到达 `nums[n-1]`

---

思路：看题解，还是贪心算法：这个思路挺难理解的

```java
    /**
     * @param nums
     * @return 45. 跳跃游戏 II
     * 思路：先计算第一步能到达的最右边，记为第一步；
     * 第一步能到达的范围内计算所有的max(num[i]+i),即为第二步能到达的最右边界，步数+1
     * 以此类推，直到最右边界超过最后一个
     *
     * [2,3,1,1,4]
     */
    public static int jump(int[] nums) {
        int length = nums.length;
        int count = 0;
        //当前步能到达的最大坐标
        int maxRight = 0;
        //从当前步能到达的下一步的最大坐标
        int nextMaxRight = 0;
        for (int i = 0; i < length-1; i++) {
            //当前步能移动的最大距离
            nextMaxRight = Math.max(nextMaxRight, nums[i] + i);
            if (i == maxRight) {
                if (maxRight >= length-1 ) {
                    break;
                }
                count++;
                //否则计算下一步能到达的最大坐标
                maxRight = nextMaxRight;
            }
        }
        return count;
    }
```









## [763. 划分字母区间](/https://leetcode.cn/problems/partition-labels/?envType=study-plan-v2&id=top-100-liked)

中等

给你一个字符串 `s` 。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。

注意，划分结果需要满足：将所有划分结果按顺序连接，得到的字符串仍然是 `s` 。

返回一个表示每个字符串片段的长度的列表。

 

**示例 1：**

**示例 2：**

```
输入：s = "eccbbbbdec"
输出：[10]
```

 

**提示：**

- `1 <= s.length <= 500`
- `s` 仅由小写英文字母组成

---

```
    /**
     * @param s 763. 划分字母区间
     * @return 思路：由于每个字母只能出现在同一个片段中，所以从头开始变了每个字符
     * 当遍历第一个字符x时，则到最后出现x的这一段必然是同一段，确定了一个start和end
     * 继续遍历第二个字符，判断end是否大于当前end，是的话扩容，直到判断到了end位置，就切割一个片段
     * 下一次从end+1的位置继续判断
     */
    public static List<Integer> partitionLabels(String s) {
        int start = 0;
        int end = 0;
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i <s.length(); i++) {
            end = Math.max(s.lastIndexOf(s.charAt(i)),end);
            if (i ==end){
                res.add(end-start+1);
                start = end +1;
            }
        }
        return res;
    }
```

优化：将每个字母出现的最后位置放在int[26]中，避免每次通过0(n)的复杂度查找

```
    public static List<Integer> partitionLabels2(String s) {
        int length = s.length();
        int[] chars = new int[26];
        for (int i = 0; i < length; i++) {
            chars[(s.charAt(i)-'a')] = i;
        }

        int start = 0;
        int end = 0;
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i <s.length(); i++) {
            end = Math.max(chars[s.charAt(i)-'a'],end);
            if (i ==end){
                res.add(end-start+1);
                start = end +1;
            }
        }
        return res;
    }
```



# 动态规划



## [509. 斐波那契数](https://leetcode-cn.com/problems/fibonacci-number/)

难度简单

**斐波那契数** （通常用 `F(n)` 表示）形成的序列称为 **斐波那契数列** 。该数列由 `0` 和 `1` 开始，后面的每一项数字都是前面两项数字的和。也就是：

```
F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
```

给定 `n` ，请计算 `F(n)` 。

 

**示例 1：**

```
输入：n = 2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1
```

**示例 2：**

```
输入：n = 3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2
```

**示例 3：**

```
输入：n = 4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3
```

 

**提示：**

- `0 <= n <= 30`

---

经典的动态规划：

```java
    public int fib(int n) {
        if(n==0||n==1){
            return n;
        }
        
        int[] dp = new int [n+1];
        dp[0] = 0;
        dp[1] = 1;
        
        for(int i = 2;i<=n;i++){
           dp[i] = dp[i-1] + dp[i-2];
        }
        
        return dp[n];

    }
```

todo：矩阵快速幂，通项公式







## [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

难度简单

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

 

**示例 1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例 2：**

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

 

**提示：**

- `1 <= n <= 45`

------

题解：第n阶的策略可以拆分成n-1阶走一步和n-2阶走两步

即f(n) =f(n-1)+f(n-2)

n= 1  1

n=2   1+1 或者 2



动态规划：注意**n+2；** 

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



## [118. 杨辉三角](/https://leetcode.cn/problems/pascals-triangle/?envType=study-plan-v2&id=top-100-liked)

给定一个非负整数 *numRows，*生成「杨辉三角」的前 *numRows* 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

![img](https://pic.leetcode-cn.com/1626927345-DZmfxB-PascalTriangleAnimated2.gif)

 

**示例 1:**

```
输入: numRows = 5
输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

**示例 2:**

```
输入: numRows = 1
输出: [[1]]
```

 

**提示:**

- `1 <= numRows <= 30`



---

思路：动态规划，下一个只和上一个有关

```java
    public static List<List<Integer>> generate(int numRows) {
        List<Integer> list = new ArrayList<>();
        list.add(1);
        List<List<Integer>> res = new ArrayList<>();
        res.add(list);
        for (int i = 2; i <=numRows; i++) {
            List<Integer> newList=new ArrayList<>();
            newList.add(0,1);

            for (int j = 1; j < i-1; j++) {
                newList.add(list.get(j-1)+list.get(j));
            }
            newList.add(1);
            res.add(newList);
            list = newList;
        }
        return res;
    }
```

TODO:不够简洁



## [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

难度中等

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

 

**示例 1：**

```text
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 2：**

```text
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

 

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

------

```java
public int rob(int[] nums) {
        //定义dp[i] 表示第i间屋子能获得的最高金额，则分两种情况：
        //1.偷第i间屋子的钱nums[i],此时不能偷i-1间屋子，则
        //获得的最高收益是dp[i-2]+nums[i]
        //2.不偷第i间屋子的钱nums[i],则
        //获得的最高收益是dp[i-1]
        //动态转移方程是：dp[i]=Math.max(dp[i-2]+nums[i],dp[i-1])
        int length = nums.length;
        if(length==1){
             return nums[0];
         }             
        int[] dp = new int[length];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0],nums[1]);
        for(int i=2;i<length;i++){
            dp[i] = Math.max(dp[i-2]+nums[i],dp[i-1]);
        }
        return dp[length-1];
    }
```

todo:简化成 只存储前两个的值



## [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

难度中等

给你一个整数数组 `nums` ，请你找出数组中乘积最大的非空连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

测试用例的答案是一个 **32-位** 整数。

**子数组** 是数组的连续子序列。

 

**示例 1:**

```text
输入: nums = [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

**示例 2:**

```text
输入: nums = [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```

 

**提示:**

- `1 <= nums.length <= 2 * 104`
- `-10 <= nums[i] <= 10`
- `nums` 的任何前缀或后缀的乘积都 **保证** 是一个 **32-位** 整数

------

```java
    public int maxProduct(int[] nums) {
        //定义dp[i][0]为以第i个数字的乘积是正得最大的乘积
        //定义dp[i][1]为前i个数字的乘积是负数的绝对值最大的乘积

        int length = nums.length;
        int[][]dp = new int[length][2];
        dp[0][0] = nums[0];
        int max = dp[0][0];
        dp[0][1] = nums[0];
        if(length==1){
            return nums[0];
        }
        for(int i = 1;i<length;i++){
            if(nums[i]>=0){
                dp[i][0] = Math.max(dp[i-1][0]*nums[i],nums[i]);
                dp[i][1] = Math.min(dp[i-1][1]*nums[i],nums[i]);
            }else{
                dp[i][0] = Math.max(dp[i-1][1]*nums[i],nums[i]);
                dp[i][1] = Math.min(dp[i-1][0]*nums[i],nums[i]);
            }
            max = Math.max(max,dp[i][0]);
        }
        return max;
    }
```

第二次：滚动数组

```
    /**
     * @param nums
     * @return 152. 乘积最大子数组
     * [2,3,-2,4]
     * 遍历数组，假定以nums[i]为结尾的最大子数组值为dp[i];分为两种情况：
     * nums[i]>=0,则最大值为dp[i-1][正数]*nums[i]
     * nums<0，则最大值为dp[i-1][负数]*nums[i]
     * 注意考虑值为0的情况
     *[-4,-3,-2]
     */
    public static int maxProduct(int[] nums) {
        //最大正数乘积
        int maxPositive = 0;
        //最小负数乘积
        int minNegative = 0;
        int ans = 0;
        if (nums.length==1){
            return nums[0];
        }
        for (int i = 0; i < nums.length; i++) {
            if (nums[i]>=0){
                maxPositive = Math.max(maxPositive*nums[i],nums[i]);
                minNegative = Math.min(minNegative*nums[i],nums[i]);
            }else {
                //注意这里
                int temp = maxPositive;
                maxPositive = Math.max(minNegative*nums[i],nums[i]);
                minNegative = Math.min(temp*nums[i],nums[i]);
            }
            ans = Math.max(maxPositive,ans);
            System.out.println(maxPositive+"==="+minNegative+"==="+ans);
        }
        return ans;
    }
```



## [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

难度中等

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

**示例 1：**

```text
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例 2：**

```text
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例 3：**

```text
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

 

**提示：**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

 

**进阶：**

- 你能将算法的时间复杂度降低到 `O(n log(n))` 吗?



------

```java
    public int lengthOfLIS(int[] nums) {
        //定义dp[i]表示以第i个数结尾的（包含nums[i])的最长子序列长度，
        //则dp[i]= nums[i]>nums[j](0<=j<i)的最大dp[j]+1，没有nums[i]>nums[j]，则dp[i]=1
        //取出dp[i]的最大值
        int maxNum = 1;
        int length = nums.length;
        int[] dp = new int[length];
        dp[0] = 0;
        for (int i = 0; i < length ; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i]>nums[j]){
                    dp[i] = Math.max(dp[i],dp[j]);
                }
            }
            maxNum = Math.max(maxNum,++dp[i]);
        }
        return maxNum;
    }
```

执行用时：65 ms, 在所有 Java 提交中击败了23.68%的用户

内存消耗：37.5 MB, 在所有 Java 提交中击败了99.37%的用户

todo:降低时间复杂度的算法



## [416. 分割等和子集](/https://leetcode.cn/problems/partition-equal-subset-sum/?envType=study-plan-v2&id=top-100-liked)

中等

给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

 

**示例 1：**

```
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```

**示例 2：**

```
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集。
```

 

**提示：**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

---

思路：没有，看题解：背包？

```
    /**
     * @param nums 416. 分割等和子集
     * @return 即：从nums数组中找到一组数的和刚好==所有数和的一半，target
     * 因此：所有数和必须是偶数，
     * 同时，当nums中有>target的，则显然false
     * 用动态规划的思想转换成：依次计算前i个数中能否找出一组数满足和为target
     * 这个问题又能拆分成两种情况：即
     * 1）从前i-1个数能找出，则不选择nums[i]也能
     * 2)前i-1个数不能，则判断前i-1个数能否找到和为target-num[i]的，选择nums[i]也能满足,因为target-nums[i]>=0
     * 因此当target<nums[i]时，只能选择1）
     */
    public static boolean canPartition(int[] nums) {
        int length = nums.length;
        int sum = 0;
        int max = 0;
        for (int num : nums) {
            sum += num;
            max = Math.max(max, num);
        }
        int target = sum / 2;
        if (sum % 2 != 0 || max > target) {
            return false;
        }
        //dp表示从nums[i]共length个数找到和为0到target（共target+1个）
        boolean[][] dp = new boolean[length][target + 1];
        //当target=0时，dp[i][0]显然为true
        for (int i = 0; i < dp.length; i++) {
            dp[i][0] = true;
        }
        //从前一个数中只能找到一个target为nums[0]的，即dp[0][其他值]都是false
        dp[0][nums[0]] = true;
        //这样第一行和第一列的值都有了
        for (int i = 1; i < length; i++) {
            for (int j = 1; j <= target; j++) {
                if (nums[i] > j) {
                    dp[i][j] = dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j] | dp[i - 1][j - nums[i]];
                }
            }
        }
        return dp[length - 1][target];
    }
```









# 技巧



## [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

难度简单

给你一个 **非空** 整数数组 `nums` ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

 

**示例 1 ：**

```
输入：nums = [2,2,1]
输出：1
```

**示例 2 ：**

```
输入：nums = [4,1,2,1,2]
输出：4
```

**示例 3 ：**

```
输入：nums = [1]
输出：1
```

 

**提示：**

- `1 <= nums.length <= 3 * 104`
- `-3 * 104 <= nums[i] <= 3 * 104`
- 除了某个元素只出现一次以外，其余每个元素均出现两次。

------

哈希法：

```java
    public static int singleNumber(int[] nums) {

        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            if (!set.add(num)) {
                set.remove(num);
            }
        }
        return set.iterator().next();
    }
```



异或 ：a^a=0,a^0=a

```java
public static int singleNumber3(int[] nums) {

    int result = 0;
    for (int num : nums) {
        result = result ^ num;
    }
    return result;
}
```



关于位运算：https://www.cnblogs.com/shuaiding/p/11124974.html



## [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

难度简单

给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

**示例 1：**

```
输入：nums = [3,2,3]
输出：3
```

**示例 2：**

```
输入：nums = [2,2,1,1,1,2,2]
输出：2
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 5 * 104`
- `-109 <= nums[i] <= 109`

------

思路：

有一个摩尔投票法

形象解释：

诸侯乱战，其中有一方诸侯A的人数超过总人数的一半，这样不是相同一方的1v1抵消，则最后剩下的一定是人数最多的那一方诸侯，最差的是所有其他诸侯联合打A，
但是还是A会赢

```java
    public static int majorityElement(int[] nums) {
        int majority =nums[0];
        int count = 1;
        for (int i = 1;i<nums.length;i++) {
            if (count==0){
                majority = nums[i];
                count++;
            }else {
                if (majority==nums[i]){
                    count++;
                }else {
                    count--;
                }
            }
        }
        return majority;
    }
```





# 未知



## [448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

难度简单

给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, n] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

示例:

输入: [4,3,2,7,8,2,3,1]  输出: [5,6] 

`---

思路：如果将[1,n]带入数组验证是否存在，则时间复杂度是n²，显然不行



题解：鸽舍原理，原地修改数组



```java
public static List<Integer> findDisappearedNumbers(int[] nums) {

    for (int i = 0; i < nums.length; i++) {
        int val = nums[i];
        if (val > 0) {
            if (nums[val - 1] > 0) {
                nums[val - 1] = -nums[val - 1];
            }
        } else {
            if (nums[-val - 1] > 0) {
                nums[-val - 1] = -nums[-val - 1];
            }
        }
    }
    List<Integer> list = new ArrayList<>();
    for (int j = 0; j < nums.length; j++) {
        if (nums[j] > 0) {
            list.add(j + 1);
        }
    }
    return list;
}
```

执行用时：6 ms, 在所有 Java 提交中击败了61.30%的用户

内存消耗：47.3 MB, 在所有 Java 提交中击败了69.66%的用户

或者可以将对应的位置加上n，之后比较是否大于n，不大于的表示当前位置没有被加过，所以对应的i就是没有

```java
public static List<Integer> findDisappearedNumbers2(int[] nums) {

    int length = nums.length;
    for (int num : nums) {
        int val = (num-1)%length;
        nums[val]+=length;
    }
    List<Integer> list = new ArrayList<>();
    for (int j = 0; j < nums.length; j++) {
        if (nums[j] <= length) {
            list.add(j + 1);
        }
    }
    return list;
}
```





