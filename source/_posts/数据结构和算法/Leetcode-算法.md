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





## [15. 三数之和](/https://leetcode.cn/problems/3sum/description/)

中等

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请

你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

 

**提示：**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`



---

TODO:还需练习 

```
    public static List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        int length = nums.length;
        int maxRight;
        for (int i = 0; i <= length - 3 && nums[i] <= 0; i++) {
            //需要nums[i]<=0
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            maxRight = length - 1;
            int target = -nums[i];

            for (int left = i + 1; left <= length - 2; left++) {
                if (left > i + 1 && nums[left] == nums[left - 1]) {
                    continue;
                }
                int right = maxRight;
                while (left < right && nums[left] + nums[right] > target) {
                    right--;
                }
                if (left == right) {
                    break;
                }
                maxRight = right;
                //超出时间的原因是只有当和为0时，才将right的范围缩小
                //实际上：应该当他们的和》0时，也应该缩小right的范围
                //当left++后值变大，和只能更大，所以right要缩小范围
                if (nums[left] + nums[right] == 0) {
                    List<Integer> list = new ArrayList<>();
                    list.add(nums[i]);
                    list.add(nums[left]);
                    list.add(nums[right]);
                    result.add(list);
                }
            }
        }
        return result;
    }
```



## [11. 盛最多水的容器](/https://leetcode.cn/problems/container-with-most-water/?envType=study-plan-v2&envId=top-100-liked)

中等

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

 

**示例 1：**

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```

 

**提示：**

- `n == height.length`
- `2 <= n <= 105`
- `0 <= height[i] <= 104`

---

思路：超时后，理解双指针的正确性

解法一：优化后的双重for：超时

```
    public int maxArea(int[] height) {
        int length = height.length;
        int maxArea = 0;
        for (int i = 0; i < length-1 ; i++) {
            int maxRight = length-1;
            int maxHeight = height[length-1];
            for (int j = length-1; j >i ; j--) {
                if (height[j]>maxHeight){
                    //判断面积
                    int area1 = Math.min(height[i], height[j])*(j-i);
                    int area2 = Math.min(height[i], maxHeight)*(maxRight-i);
                    if (area1>area2){
                        maxRight = j;
                        maxHeight = height[j];
                    }
                }
            }
            maxArea = Math.max(maxArea,Math.min(height[i], maxHeight)*(maxRight-i));
            System.out.println(i+"==="+maxArea);
        }
        return maxArea;
    }
```



```
    public int maxArea2(int[] height) {
        int length = height.length;
        int maxArea = 0;
        int left = 0;
        int right = length - 1;
        while (left < right) {
            int minHeight = Math.min(height[left] , height[right]);
            maxArea = Math.max(maxArea, minHeight * (right - left));
            if (height[left] < height[right]) {
                ++left;
            } else {
                --right;
            }

        }
        return maxArea;
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



##  [53. 最大子数组和todo](https://leetcode-cn.com/problems/maximum-subarray/)

中等



给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

 

**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例 2：**

```
输入：nums = [1]
输出：1
```

**示例 3：**

```
输入：nums = [5,4,-1,7,8]
输出：23
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

 

**进阶：**如果你已经实现复杂度为 `O(n)` 的解法，尝试使用更为精妙的 **分治法** 求解。

---





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



## [189. 轮转数组](/https://leetcode.cn/problems/rotate-array/?envType=study-plan-v2&envId=top-100-liked)

中等

给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

 

**示例 1:**

```
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]
```

**示例 2:**

```
输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释: 
向右轮转 1 步: [99,-1,-100,3]
向右轮转 2 步: [3,99,-1,-100]
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`
- `0 <= k <= 105`

 

**进阶：**

- 尽可能想出更多的解决方案，至少有 **三种** 不同的方法可以解决这个问题。
- 你可以使用空间复杂度为 `O(1)` 的 **原地** 算法解决这个问题吗？



---

方法一：新建数组

```
    public static void rotate(int[] nums, int k) {
        int length = nums.length;
        int[] ans = new int[length];
        for (int i = 0; i < length; i++) {
            int n = (i + k) % length;
            ans[n] = nums[i];
        }
//        nums = Arrays.copyOf(ans,length);
//        nums = ans;
        System.arraycopy(ans, 0, nums, 0, length);
    }
```

方法二：反转数组

```
    public static void rotate2(int[] nums, int k) {
        int length = nums.length;
        int n = k % length;
        reverse(nums, 0, length-1);
        reverse(nums,0,n-1);
        reverse(nums,n,length-1);
    }
    private static void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }    
```



## [238. 除自身以外数组的乘积](/https://leetcode.cn/problems/product-of-array-except-self/description/)

中等

给你一个整数数组 `nums`，返回 *数组 answer ，其中 answer[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积* 。

题目数据 **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在  **32 位** 整数范围内。

请**不要使用除法，**且在 `O(*n*)` 时间复杂度内完成此题。

 

**示例 1:**

```
输入: nums = [1,2,3,4]
输出: [24,12,8,6]
```

**示例 2:**

```
输入: nums = [-1,1,0,-3,3]
输出: [0,0,9,0,0]
```

 

**提示：**

- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在  **32 位** 整数范围内

 

**进阶：**你可以在 `O(1)` 的额外空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组**不被视为**额外空间。）



---



```
    public int[] productExceptSelf(int[] nums) {
        int length = nums.length;
        int L[] = new int[length];
        int R[] = new int[length];
        int ans[] = new int[length];
        L[0] = 1;
        R[length - 1] = 1;
/*        for (int i = 1; i < length; i++) {
            L[i] = L[i - 1] * nums[i];
            R[length - i - 1] = R[length - i] * nums[length - i - 1];
        }*/
        for (int i = 1; i < length; i++) {
            L[i] = L[i - 1] * nums[i - 1];
        }
        for (int i = length - 2; i >= 0; i--) {
            R[i] = R[i + 1] * nums[i + 1];
        }
        for (int i = 0; i < length; i++) {
            ans[i] = L[i] * R[i];
        }
        return ans;
    }
```



# 矩阵

## [73. 矩阵置零](/https://leetcode.cn/problems/set-matrix-zeroes/description/?envType=study-plan-v2&envId=top-100-liked)
中等






```
    public void setZeroes(int[][] matrix) {
        int rows = matrix.length;
        int columns = matrix[0].length;
        boolean flagRow0 = false;
        boolean flagColumn0 = false;
        for (int i = 0; i < rows; i++) {
            if (matrix[i][0] == 0) {
                flagColumn0 = true;
            }
        }
        for (int j = 0; j < columns; j++) {
            if (matrix[0][j] == 0) {
                flagRow0 = true;
            }
        }
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                    continue;
                }
            }
        }
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < columns; j++) {
                if (matrix[0][j] == 0 || matrix[i][0] == 0) {
                    matrix[i][j] = 0;
                }
            }

        }
        if (flagRow0) {
            for (int j = 0; j < columns; j++) {
                matrix[0][j] = 0;
            }
        }
        if (flagColumn0) {
            for (int i = 0; i < rows; i++) {
                matrix[i][0] = 0;
            }
        }
    }
```



# 链表

## [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

难度简单

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

**自定义评测：**

**评测系统** 的输入如下（你设计的程序 **不适用** 此输入）：

- `intersectVal` - 相交的起始节点的值。如果不存在相交节点，这一值为 `0`
- `listA` - 第一个链表
- `listB` - 第二个链表
- `skipA` - 在 `listA` 中（从头节点开始）跳到交叉节点的节点数
- `skipB` - 在 `listB` 中（从头节点开始）跳到交叉节点的节点数

评测系统将根据这些输入创建链式数据结构，并将两个头节点 `headA` 和 `headB` 传递给你的程序。如果程序能够正确返回相交节点，那么你的解决方案将被 **视作正确答案** 。

 

**示例 1：**

[![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```text
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,6,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

**示例 2：**

[![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)

```text
输入：intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Intersected at '2'
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [1,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

**示例 3：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```text
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。
```

 

**提示：**

- `listA` 中节点数目为 `m`
- `listB` 中节点数目为 `n`
- `1 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`
- `0 <= skipA <= m`
- `0 <= skipB <= n`
- 如果 `listA` 和 `listB` 没有交点，`intersectVal` 为 `0`
- 如果 `listA` 和 `listB` 有交点，`intersectVal == listA[skipA] == listB[skipB]`

 

**进阶：**你能否设计一个时间复杂度 `O(m + n)` 、仅用 `O(1)` 内存的解决方案？

------

方法一：遍历链表1，将节点存到HashSet中，遍历链表2，看节点是否在hashSet中，在则相交

```
    public ListNode getIntersectionNode1(ListNode headA, ListNode headB) {
        Set<ListNode> nodeSet = new HashSet<>();
        ListNode temp = headA;
        while (temp!=null){
            nodeSet.add(temp);
            temp =temp.next;
        }
        temp = headB;
        while (temp!=null){
            if (nodeSet.contains(temp)){
                return temp;
            }else {
                temp = temp.next;
            }
        }
        return null;
    }
```

时间：m+n；空间：m



方法二：

```java
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode preA = headA;
        ListNode preB = headB;
        while (preA != preB) {
            preA = preA == null ? headB : preA.next;
            preB = preB == null ? headA : preB.next;
        }
        return preA;
    }
```
时间：m+n;空间：1



## [206. 反转链表todo](https://leetcode-cn.com/problems/reverse-linked-list/)



难度简单

给你单链表的头节点  

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```text
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```text
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```text
输入：head = []
输出：[]
```

 

**提示：**

- 链表中节点的数目范围是 `[0, 5000]`
- `-5000 <= Node.val <= 5000`

 

**进阶：**链表可以选用迭代或递归方式完成反转。你能否用两种方法解决这道题？

------

方法一：利用中间变量依次反转，注意返回的事pre（迭代）

```
    public ListNode reverseList3(ListNode head) {
        ListNode pre = null;
        ListNode curr = head;

        while (curr!=null){
            ListNode next = curr.next;
            curr.next=pre;
            pre = curr;
            curr =next;
        }
        return pre;
    }
```



方法二：递归todo很难理解





## [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)



难度简单

给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```text
输入：head = [1,2,2,1]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```text
输入：head = [1,2]
输出：false
```

 

**提示：**

- 链表中节点数目在范围`[1, 105]` 内
- `0 <= Node.val <= 9`

 

**进阶：**你能否用 `O(n)` 时间复杂度和 `O(1)` 空间复杂度解决此题？

------

思路：

如果不考虑空间，将原链表反转得到的新链表，依次比较即可。

利用快慢指针找到中间位置，将后半部分反转，

比较前半部分是否和后半部分相同

```java
public boolean isPalindrome(ListNode head) {
        if (head == null) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        //此时无论是有技术还是偶数个 slow.next就是后半段的起点
        ListNode middle = slow.next;
        ListNode halfLeft = reverseList2(middle);
        while (halfLeft !=null) {
            if (head.val != halfLeft.val) {
                return false;
            } else {
                head = head.next;
                halfLeft = halfLeft.next;
            }
        }
        return true;
    }
    public static ListNode reverseList2(ListNode head) {

        ListNode pre = null;
        ListNode curr = head;

        while (curr != null) {
            ListNode next = curr.next;
            curr.next = pre;
            pre = curr;
            curr = next;
        }
        return pre;
    }    
```



todo：还有种递归算法



## [141. 环形链表(完)](https://leetcode-cn.com/problems/linked-list-cycle/)

难度简单

给你一个链表的头节点 `head` ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。**注意：pos 不作为参数进行传递** 。仅仅是为了标识链表的实际情况。

*如果链表中存在环* ，则返回 `true` 。 否则，返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

```text
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```text
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```text
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

 

**提示：**

- 链表中节点的数目范围是 `[0, 104]`
- `-105 <= Node.val <= 105`
- `pos` 为 `-1` 或者链表中的一个 **有效索引** 。

 

**进阶：**你能用 `O(1)`（即，常量）内存解决此问题吗？

------

思路：因为o(1)，所有不能用HashSet来判断

快慢指针

方法二：题解中的快慢指针有环时一定相遇

```java
if (head==null||head.next==null){
    return false;
}
ListNode slow = head;
ListNode fast = head.next;
while (slow!=fast){
    if (fast==null||fast.next==null){
        return false;
    }
    slow = slow.next;
    fast = fast.next.next;
}
return true;
```



## [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

难度中等

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 null。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：pos 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```text
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```text
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```text
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

**提示：**

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引

**进阶：**你是否可以使用 `O(1)` 空间解决此题？

------

重点是理解，双指针是从head开始走

![Picture1.png](https://pic.leetcode-cn.com/a4788076d4f3ad247c2023f92bb1585d05c5132ece7ed1205e2e171e25648adc-Picture1.png)



```
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null) {
            if (fast.next == null) {
                return null;
            }
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                ListNode start = head;
                while (start != slow) {
                    start = start.next;
                    slow = slow.next;
                }
                return start;
            }

        }
        return null;
    }
```







## [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

难度简单

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```text
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例 2：**

```text
输入：l1 = [], l2 = []
输出：[]
```

**示例 3：**

```text
输入：l1 = [], l2 = [0]
输出：[0]
```

 

**提示：**

- 两个链表的节点数目范围是 `[0, 50]`
- `-100 <= Node.val <= 100`
- `l1` 和 `l2` 均按 **非递减顺序** 排列

------

方法一：

```java
    public ListNode mergeTwoLists2(ListNode list1, ListNode list2) {
        ListNode head = new ListNode();
        ListNode ans = head;
        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                ans.next = list1;
                list1 = list1.next;
            } else {
                ans.next = list2;
                list2 = list2.next;
            }
            ans = ans.next;
        }
        ans.next = list1 == null ? list2 : list1;
        return head.next;
    }
```



方法二：todo递归法：待理解

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if (l1 == null) {
        return l2;
    } else if (l2 == null) {
        return l1;
    } else if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    } else {
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
}
```





## [2. 两数相加todo](https://leetcode-cn.com/problems/add-two-numbers/)

难度中等

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/02/addtwonumber1.jpg)

```text
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

**示例 2：**

```text
输入：l1 = [0], l2 = [0]
输出：[0]
```

**示例 3：**

```text
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

 

**提示：**

- 每个链表中的节点数在范围 `[1, 100]` 内
- `0 <= Node.val <= 9`
- 题目数据保证列表表示的数字不含前导零

------

思路：链表 双指针 取余

脑会了手不会

注意最后一位是否需要进一位，定一个两个指针，一个指头用来返回，一个指尾用来计算

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {

    ListNode head = null;
    ListNode tail = null;
    int a = 0;
    while (l1 != null || l2 != null) {

        int n1 = l1 == null ? 0 : l1.val;
        int n2 = l2 == null ? 0 : l2.val;
        int plus = n1 + n2 + a;
        if (head == null) {
            tail = head = new ListNode(plus % 10);
        } else {
            tail.next = new ListNode(plus % 10);
            tail = tail.next;
        }
        a = plus / 10;
        l1 = l1 == null ? null : l1.next;
        l2 = l2 == null ? null : l2.next;
    }
    if (a>0){
        tail.next = new ListNode(a);
    }

    return head;
}
```





## [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

难度中等
给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]

**示例 2：**

输入：head = [1], n = 1
输出：[]

**示例 3：**

输入：head = [1,2], n = 1
输出：[1]

**提示：**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

**进阶：**你能尝试使用一趟扫描实现吗？

------

思路：一次扫描要找到倒数第n个节点，需要用到快慢`双指针`

```java
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode pre = new ListNode();
        pre.next = head;
        ListNode fast = pre,slow =pre;
        for (int i = 0; i < n; i++) {
            fast = fast.next;
        }
        while (fast.next!=null){
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        // 考虑到只有一个元素的情况下 head可能会被移除
        return pre.next;
    }
```



# 二叉树

## [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

难度简单

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

**示例：**
给定二叉树 `[3,9,20,null,null,15,7]`，

```text
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 。

------

思路：没有，看题解

```java

public int maxDepth(TreeNode root) {
    if (root==null){
        return 0;
    }
    return Math.max(maxDepth(root.left),maxDepth(root.right))+1;
}
```



todo 广度优先算法



## [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

难度简单

给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

输入：root = [2,1,3]
输出：[2,3,1]

**示例 3：**

输入：root = []
输出：[]

**提示：**

- 树中节点数目范围在 `[0, 100]` 内
- `-100 <= Node.val <= 100`

------

思路：

把每一个节点的左右节点互调

与[**101. 对称二叉树**](https://leetcode-cn.com/problems/symmetric-tree/)不同的是将root放入队列中并且比较的是

!queue.isEmpty()

迭代：

```java
    public TreeNode invertTree(TreeNode root) {
        if(root==null){
            return root;
        }
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()){
            TreeNode treeNode = queue.removeFirst();
            TreeNode temp = treeNode.left;
            treeNode.left = treeNode.right;
            treeNode.right =temp;
            if (treeNode.left!=null){
                queue.add(treeNode.left);
            }
            if (treeNode.right!=null){
                queue.add(treeNode.right);
            }
        }
        return root;
    }
```



递归：

```java
    public TreeNode invertTree(TreeNode root) {
        if (root ==null){
            return null;
        }
        TreeNode temp = root.left;
        root.left = root.right;
        root.right =temp;
        invertTree(root.left);
        invertTree(root.right);

        return root;
    }
```





## [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

难度简单

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

输入：root = [1,2,2,3,4,4,3]
输出：true

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

输入：root = [1,2,2,null,3,null,3]
输出：false

**提示：**

- 树中节点数目在范围 `[1, 1000]` 内
- `-100 <= Node.val <= 100`

**进阶：**你可以运用递归和迭代两种方法解决这个问题吗？

------

思路：题解

迭代：

```java
    public boolean isSymmetric2(TreeNode root) {
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(root.left);
        queue.add(root.right);
        while (queue.size() > 0) {
            TreeNode left = queue.removeFirst();
            TreeNode right = queue.removeFirst();
            if (left == null && right == null) {
                continue;
            }
            if (left == null || right == null || left.val != right.val) {
                return false;
            }
            queue.add(left.left);
            queue.add(right.right);
            queue.add(left.right);
            queue.add(right.left);
        }
        return true;
    }
```



递归：

```java
    public boolean isSymmetric(TreeNode root) {
        return isSymmetric(root, root);
    }

    public boolean isSymmetric(TreeNode left, TreeNode right) {
        if (left == null && right == null) {
            return true;
        }
        if (left == null || right == null) {
            return false;
        }
        if (left.val == right.val) {
            return isSymmetric(left.left, right.right) && isSymmetric(left.right, right.left);
        } else {
            return false;
        }
    }
```



## [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

难度简单

给你一棵二叉树的根节点，返回该树的 **直径** 。

二叉树的 **直径** 是指树中任意两个节点之间最长路径的 **长度** 。这条路径可能经过也可能不经过根节点 `root` 。

两节点之间路径的 **长度** 由它们之间边数表示。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
输入：root = [1,2,3,4,5]
输出：3
解释：3 ，取路径 [4,2,1,3] 或 [5,2,1,3] 的长度。
```

**示例 2：**

```
输入：root = [1,2]
输出：1
```

 

**提示：**

- 树中节点数目在范围 `[1, 104]` 内
- `-100 <= Node.val <= 100`

------

思路：没有 看题解

以每个节点为根节点的左右子树的最大深度之和

```java

    /**
     * @param root
     * @return 543. 二叉树的直径
     */
    //定义最长路径
    // 刚好=左子树的最大高度+右子树的最大高度
    int ans = 0;

    public int diameterOfBinaryTree(TreeNode root) {

        depth(root);
        return ans;
    }

    /**
     * @param node 迭代
     * @return 求当前节点的最大深度，（=左右子树的最大深度之和+1）
     * 并通过左右子树的最大深度比较更新总的最大深度
     * 重点还是求树的最大高度
     */
    public int depth(TreeNode node) {
        if (node == null) {
            return 0;
        }
        int L = depth(node.left);
        int R = depth(node.right);
        ans = Math.max(ans, L + R + 1);
        return Math.max(L, R) + 1;
    }
```





## [144. 二叉树的前序遍历](/https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)

给你二叉树的根节点 `root` ，返回它节点值的 **前序** 遍历。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```
输入：root = [1,null,2,3]
输出：[1,2,3]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [1]
输出：[1]
```

**示例 4：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_5.jpg)

```
输入：root = [1,2]
输出：[1,2]
```

**示例 5：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_4.jpg)

```
输入：root = [1,null,2]
输出：[1,2]
```

 

**提示：**

- 树中节点数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

 

**进阶：**递归算法很简单，你可以通过迭代算法完成吗？



---

方法一：递归



```
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorderTraversal(root, result);
        return result;
    }

    public void preorderTraversal(TreeNode root, List<Integer> list) {
        if (root != null) {
            list.add(root.val);
            preorderTraversal(root.left, list);
            preorderTraversal(root.right, list);
        }
    }
```

迭代：利用栈

```

    public List<Integer> preorderTraversal2(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode pop = stack.pop();
            if (pop != null) {
                result.add(pop.val);
                stack.push(pop.right);
                stack.push(pop.left);
            }
        }
        return result;
    }
```

方法三：



## [102. 二叉树的层序遍历](/https://leetcode.cn/problems/binary-tree-level-order-traversal/?envType=study-plan-v2&envId=top-100-liked)

中等

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

 

**提示：**

- 树中节点数目在范围 `[0, 2000]` 内
- `-1000 <= Node.val <= 1000`

---

思路：用队列记录：遍历一个节点后将它的左右子节点放入队列中

```
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        if (root == null) {
            return ans;
        }
        Deque<TreeNode> deque = new LinkedList<>();
        deque.add(root);
        while (!deque.isEmpty()) {
            int size = deque.size();
            List<Integer> list = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = deque.removeFirst();
                if (node.left != null) {
                    deque.add(node.left);
                }
                if (node.right != null) {
                    deque.add(node.right);
                }
                list.add(node.val);
            }
            ans.add(list);
        }
        return ans;
    }
```












# 回溯

## [257. 二叉树的所有路径](/https://leetcode.cn/problems/binary-tree-paths/)

简单

给你一个二叉树的根节点 `root` ，按 **任意顺序** ，返回所有从根节点到叶子节点的路径。

**叶子节点** 是指没有子节点的节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```

**示例 2：**

```
输入：root = [1]
输出：["1"]
```

 

**提示：**

- 树中节点的数目在范围 `[1, 100]` 内
- `-100 <= Node.val <= 100`



---

思路：无，查出所有的可能的情况，第一个回溯算法







## [46. 全排列](https://leetcode.cn/problems/permutations/)

难度中等

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

 

**提示：**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有整数 **互不相同**



------

思路：无，看题解，回溯算法





## [78. 子集](https://leetcode-cn.com/problems/subsets/)

难度中等

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

 

**示例 1：**

```text
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```text
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**

------

思路：



初始[]

第 1 个 元素 子集 [1]

第 2个 元素 子集[2], [1,2]

第 3个 元素 子集[3],[1,3],[2.3], [1,2,3]

之后每一个元素在前一个元素子集的基础上加上当前的元素

递归法

```java
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        result.add(new ArrayList<>());
        for (int num : nums) {
            List<List<Integer>> subsetLists = new ArrayList<>();
            for (List<Integer> subsetList : result) {
                List<Integer> newSubList = new ArrayList<>(subsetList);
                newSubList.add(num);
                subsetLists.add(newSubList);
            }
            result.addAll(subsetLists);
        }
        return result;
    }
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：41.5 MB, 在所有 Java 提交中击败了5.29%的用户



todo引用被更改,见testListList

```java
/**
 * @param nums
 * @return todo 错的 引用被更改,见testListList
 */
public static List<List<Integer>> subsets2(int[] nums) {

    List<List<Integer>> result = new ArrayList<>();
    result.add(new ArrayList<>());
    for (int num : nums) {
        List<List<Integer>> subsetLists = new ArrayList<>(result);
        for (List<Integer> subsetList : subsetLists) {
            subsetList.add(num);
        }
        result.addAll(subsetLists);
    }
    return result;
}
public static void  testListList(){

    List<Integer> list1 = new ArrayList<>();
    list1.add(1);
    list1.add(2);
    //1,2
    System.out.println(list1);
    List<Integer> list2 = new ArrayList<>(list1);
    list2.add(3);
    //1,2,3
    System.out.println(list2);
    //1,2 list1没有被更改
    System.out.println(list1);
    List<List<Integer>> lists1 = new ArrayList<>();
    lists1.add(list1);
    lists1.add(list2);
    //[[1, 2], [1, 2, 3]]
    System.out.println(lists1);
    List<List<Integer>> lists2 = new ArrayList<>(lists1);
    for (List<Integer> list : lists2) {
        list.add(4);
    }
    //[[1, 2, 4], [1, 2, 3, 4]]
    System.out.println(lists2);
    //[[1, 2, 4], [1, 2, 3, 4]] lists1被更改？？
    System.out.println(lists1);
}
```











# 二分查找



# 栈

## [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

难度简单

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。

 

**示例 1：**

```
输入：s = "()"
输出：true
```

**示例 2：**

```
输入：s = "()[]{}"
输出：true
```

**示例 3：**

```
输入：s = "(]"
输出：false
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成

------

思路：遇到左括号进栈，遇到右括号，出对应的左括号的栈，看是否成功

引申：java中栈类的api和使用

```
    public static boolean isValid2(String s) {
        int length = s.length();
        if (length % 2 != 0) {
            return false;
        }
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            if (c == '{' || c == '(' || c == '[') {
                stack.push(c);
            } else {
                if (!stack.isEmpty()) {
                    return false;
                }
                Character pop = stack.pop();
                if (c == '}' && pop != '{') {
                    return false;
                } else if (c == ')' && pop != '(') {
                    return false;
                } else if (c == ']' && pop != '[') {
                    return false;
                }
            }
        }
        return stack.isEmpty();
    }
```



```
    public static boolean isValid2(String s) {
        Stack<Character> stack = new Stack<>();
        if (s.length() % 2 != 0) {
            return false;
        }
        char[] chars = s.toCharArray();
        for (char aChar : chars) {

            if (aChar == '(') {
                stack.push(')');
            } else if (aChar == '{') {
                stack.push('}');
            } else if (aChar == '[') {
                stack.push(']');
            } else {
                if (stack.empty()) {
                    return false;
                } else {
                    if (stack.pop() != aChar) {
                        return false;
                    }
                }
            }
        }

        return stack.empty();
    }
```



## [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

难度中等

- 设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

  实现 `MinStack` 类:

  - `MinStack()` 初始化堆栈对象。
  - `void push(int val)` 将元素val推入堆栈。
  - `void pop()` 删除堆栈顶部的元素。
  - `int top()` 获取堆栈顶部的元素。
  - `int getMin()` 获取堆栈中的最小元素。

   

  **示例 1:**

  ```
  输入：
  ["MinStack","push","push","push","getMin","pop","top","getMin"]
  [[],[-2],[0],[-3],[],[],[],[]]
  
  输出：
  [null,null,null,null,-3,null,0,-2]
  
  解释：
  MinStack minStack = new MinStack();
  minStack.push(-2);
  minStack.push(0);
  minStack.push(-3);
  minStack.getMin();   --> 返回 -3.
  minStack.pop();
  minStack.top();      --> 返回 0.
  minStack.getMin();   --> 返回 -2.
  ```

   

  **提示：**

  - `-231 <= val <= 231 - 1`
  - `pop`、`top` 和 `getMin` 操作总是在 **非空栈** 上调用
  - `push`, `pop`, `top`, and `getMin`最多被调用 `3 * 104` 次

------

思路：

Java中自带的Stack类，如何自己设计实现？

Stack有哪些常用的子类？

自带的能实现上面的方法，怎么实现最后一个？

要在常数时间内检索到，则必定每添加或者弹出一个元素都要对应的最小元素

https://blog.csdn.net/AsheAndWine/article/details/74036652

看题解有两种，一种是每次弹出两个元素，其中一个是最小值，另一种是辅助栈，即用另一个栈来存储最小值

执行用时：7 ms, 在所有 Java 提交中击败了79.21%的用户

内存消耗：40.3 MB, 在所有 Java 提交中击败了32.65%的用户

```java
public class MinStack {
    
    private int[] objects = new int[10];
    private int elementSize = 0;

    public MinStack() {
    }

    public void push(int x) {
        int minCapacity = elementSize + 2;
        if (objects.length < minCapacity) {
            int newLength = objects.length + objects.length;
            if (newLength < minCapacity) {
                newLength = minCapacity;
            }
            objects = Arrays.copyOf(objects, newLength);
        }
        int min = elementSize - 1 > 0 ? Math.min(objects[elementSize - 1], x) : x;
        objects[elementSize++] = x;
        objects[elementSize++] = min;
    }

    public void pop() {
        objects[elementSize-- - 1] = 0;
        objects[elementSize-- - 1] = 0;
    }

    public int top() {
        return objects[elementSize - 2];
    }

    public int getMin() {
        return objects[elementSize - 1];
    }
}
```









## [394. 字符串解码](/https://leetcode.cn/problems/decode-string/?envType=study-plan-v2&envId=top-100-liked)

中等

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 `k` ，例如不会出现像 `3a` 或 `2[4]` 的输入。

 

**示例 1：**

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

**示例 2：**

```
输入：s = "3[a2[c]]"
输出："accaccacc"
```

**示例 3：**

```
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

**示例 4：**

```
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

 

**提示：**

- `1 <= s.length <= 30`
- `s` 由小写英文字母、数字和方括号 `'[]'` 组成
- `s` 保证是一个 **有效** 的输入。
- `s` 中所有整数的取值范围为 `[1, 300]` 

---

思路：无



## [739. 每日温度](/https://leetcode.cn/problems/daily-temperatures/?envType=study-plan-v2&envId=top-100-liked)

中等

给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

 

**示例 1:**

```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```

**示例 2:**

```
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

**示例 3:**

```
输入: temperatures = [30,60,90]
输出: [1,1,0]
```

 

**提示：**

- `1 <= temperatures.length <= 105`
- `30 <= temperatures[i] <= 100`



---

思路：暴力n*n

思路：看题解：栈，递减栈

> Deque<Integer> stack = new LinkedList<Integer>();
>
> Stack<Integer> stack = new Stack<>();

TODO 

```
    /**
     * @param temperatures
     * @return 739. 每日温度
     */
    public int[] dailyTemperatures(int[] temperatures) {
        int length = temperatures.length;
        int[] result = new int[length];
        //Deque<Integer> stack = new LinkedList<Integer>();
        //这个比下面的快很多
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < length; i++) {
            while (!stack.isEmpty() && temperatures[stack.peek()] < temperatures[i]) {
                Integer pop = stack.pop();
                result[pop] = i - pop;
            }
            stack.push(i);
        }
        return result;
    }
```





# 堆

## [215. 数组中的第K个最大元素](/https://leetcode.cn/problems/kth-largest-element-in-an-array/?envType=study-plan-v2&envId=top-100-liked)

中等

给定整数数组 `nums` 和整数 `k`，请返回数组中第 `**k**` 个最大的元素。

请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

你必须设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

 

**示例 1:**

```
输入: [3,2,1,5,6,4], k = 2
输出: 5
```

**示例 2:**

```
输入: [3,2,3,1,2,4,5,5,6], k = 4
输出: 4
```

 

**提示：**

- `1 <= k <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

---

思路：排序但是时间复杂度没有O(n)的







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
        int length = nums.length;
        int[] dp = new int[length];
        dp[0] = 1;
        int longest = 0;
        for (int i = 1; i < length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            longest = Math.max(longest, dp[i]);
        }
        return longest;
    }
```



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



## [322. 零钱兑换](/https://leetcode.cn/problems/coin-change/?envType=study-plan-v2&id=top-100-liked)

中等



给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。

计算并返回可以凑成总金额所需的 **最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。

你可以认为每种硬币的数量是无限的。

 

**示例 1：**

```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```

**示例 2：**

```
输入：coins = [2], amount = 3
输出：-1
```

**示例 3：**

```
输入：coins = [1], amount = 0
输出：0
```

 

**提示：**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

---

思路：从大到小排序，依次求余数，直到为0，记录商的次数

写完后发现错了，结果不一定包括最大的，有可能只能被最小的整除；

看题解，还是动态规划，重点是怎么规划！！

```
    /**
     * @param coins  322. 零钱兑换
     * @param amount
     * @return 假设dp[i]为凑齐i金额所需要的最少硬币书
     * 那么：凑齐金额的最后一枚硬币必然是coins中的一个
     * 动态转移方程为：dp[i] = Math.min(dp[i], dp[i - coin] + 1);
     * 初始化为dp[0]=0,dp[<0]不考虑
     * 细节：初始化dp[i]为一个不可能达到的大值，这样取最小值取不到
     */
    public static int coinChange(int[] coins, int amount) {

        int length = amount + 1;
        int dp[] = new int[length];
        Arrays.fill(dp, length);
        dp[0] = 0;
        for (int i = 1; i < length; i++) {
            for (int coin : coins) {
                if (i - coin >= 0) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        return dp[amount] < length ? dp[amount] : -1;
    }
```



## [139. 单词拆分](/https://leetcode.cn/problems/word-break/description/)

中等

给你一个字符串 `s` 和一个字符串列表 `wordDict` 作为字典。请你判断是否可以利用字典中出现的单词拼接出 `s` 。

**注意：**不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

 

**示例 1：**

```
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。
```

**示例 2：**

```
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以由 "apple" "pen" "apple" 拼接成。
     注意，你可以重复使用字典中的单词。
```

**示例 3：**

```
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

 

**提示：**

- `1 <= s.length <= 300`
- `1 <= wordDict.length <= 1000`
- `1 <= wordDict[i].length <= 20`
- `s` 和 `wordDict[i]` 仅有小写英文字母组成
- `wordDict` 中的所有字符串 **互不相同**

---

思路：知道动态，不知道怎么规划，看题解，一眼就知道了！

```java
    public static boolean wordBreak(String s, List<String> wordDict) {
        Set<String> dictSet = new HashSet<>();
        for (String word : wordDict) {
            dictSet.add(word);
        }
        int length = s.length();

        boolean dp[] = new boolean[length+1];

        int start = 0;
        for (int i = 1; i <= length; i++) {
            String curr = s.substring(start, i);
            dp[i] = dictSet.contains(curr);
            if (dp[i]) {
                start = i;
            }

        }
        return dp[length];
    }
```



结果发现错了！错误原因是动态转移方程想简单了，截断会导致比如s="aaaaaaa",dic = "aaaa","aaa"返回false

```
    public static boolean wordBreak2(String s, List<String> wordDict) {
        //假设dp[i]表示字符串s中前i个字符的子串是否能被字典拆分
        //则 dp[i] = dp[j] && s.substring(j+1,i+1)在字典中
        int length = s.length();
        Set<String> dictSet = new HashSet<>(wordDict);
        boolean dp[] = new boolean[length+1];
        dp[0]=true;
        for (int i = 1; i <= length; i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j]==true){
                    dp[i]=dp[j]&&dictSet.contains(s.substring(j,i));
                    if (dp[i]){
                        break;
                    }
                }
            }
        }
        return dp[length];
    }
```



## [279. 完全平方数](/https://leetcode.cn/problems/perfect-squares/submissions/438041473/?envType=study-plan-v2&envId=top-100-liked)



给你一个整数 `n` ，返回 *和为 n 的完全平方数的最少数量* 。

**完全平方数** 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，`1`、`4`、`9` 和 `16` 都是完全平方数，而 `3` 和 `11` 不是。

 

**示例 1：**

```
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
```

**示例 2：**

```
输入：n = 13
输出：2
解释：13 = 4 + 9
```

 

**提示：**

- `1 <= n <= 104`

---

思路没错，就是速度慢，慢的原因是每次判断平方数，其实set的值只能是1,2,3...这些，不用记录直接判断就行



```
    /**
     * @param n
     * @return 279. 完全平方数
     * 用一个Set记录所有的完全平方数，m1，m2..
     * dp[n]为和为 n 的完全平方数的最少数量
     * 则dp[i]=1,如果能被开方
     * 否则，dp[i]=min{dp[i-m1],dp[i-m2]..}+1
     */
    public static int numSquares(int n) {
        Set<Integer> set = new HashSet<>();
        int[] dp = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            if (isPerfectSquare(i)) {
                set.add(i);
                dp[i] = 1;
            } else {
                int min = Integer.MAX_VALUE;
                for (Integer integer : set) {
                    min = Math.min(dp[i - integer], min);
                }
                dp[i]=min+1;
            }
        }
        return dp[n];

    }
    private static boolean isPerfectSquare(int n){
        int square = (int) Math.sqrt(n);
        return square*square==n;
    }
```



```
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp,n);
        dp[0]=0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j*j <= i; j++) {
                dp[i]= Math.min(dp[i-j*j],dp[i]);
            }
            dp[i]++;
        }
        return dp[n];
    }
```

todo还是有点慢



## [300. 最长递增子序列todo](/https://leetcode.cn/problems/longest-increasing-subsequence/?envType=study-plan-v2&envId=top-100-liked)

中等

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

 

**示例 1：**

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例 2：**

```
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例 3：**

```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

 

**提示：**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

 

**进阶：**

- 你能将算法的时间复杂度降低到 `O(n log(n))` 吗?



---



```
    /**
     * @param nums
     * @return 300. 最长递增子序列
     */
    public int lengthOfLIS(int[] nums) {
        //定义dp[i]表示以第i个数结尾的（包含nums[i])的最长子序列长度，
        //则dp[i]= nums[i]>nums[j](0<=j<i)的最大dp[j]+1，没有nums[i]>nums[j]，则dp[i]=1
        //取出dp[i]的最大值
        int length = nums.length;
        int[] dp = new int[length];
        dp[0] = 1;
        int longest = 0;
        for (int i = 1; i < length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            longest = Math.max(longest, dp[i]);
        }
        return longest;
    }
```

todo 进阶









# 多维动态规划

## [64. 最小路径和（完）](https://leetcode-cn.com/problems/minimum-path-sum/)

难度中等 

给定一个包含非负整数的 `m x n` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```text
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

**示例 2：**

```text
输入：grid = [[1,2,3],[4,5,6]]
输出：12
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `0 <= grid[i][j] <= 100`

------



```java
    public int minPathSum(int[][] grid) {
        //二维的动态规划
        //dp[i][j] 表示grid[i][j]处的最小路径值，
        //则dp[i][j] = grid[i][j]+ Math.min(dp[i-1][j],dp[i][j-1]))
        //计算边界条件dp[0][j]和dp[i][0]
        int rows = grid.length;
        int columns = grid[0].length;
        int dp[][] = new int[rows][columns];
        dp[0][0] = grid[0][0];
        for(int i = 1 ;i<rows;i++){
            dp[i][0] = dp[i-1][0] + grid[i][0];
        }
        for(int j = 1 ;j<columns;j++){
            dp[0][j] = dp[0][j-1] + grid[0][j];
        }
        for(int i = 1 ;i<rows;i++){
            for(int j = 1 ;j<columns;j++){
            dp[i][j] = grid[i][j] + Math.min(dp[i-1][j],dp[i][j-1]);
            }
        }
        return dp[rows-1][columns-1];       
    }
```





## [62. 不同路径（完）](/https://leetcode.cn/problems/unique-paths/?envType=study-plan-v2&envId=top-100-liked)

中等

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

```
输入：m = 3, n = 7
输出：28
```

**示例 2：**

```
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下
```

**示例 3：**

```
输入：m = 7, n = 3
输出：28
```

**示例 4：**

```
输入：m = 3, n = 3
输出：6
```

 

**提示：**

- `1 <= m, n <= 100`
- 题目数据保证答案小于等于 `2 * 109`

---

```
    public int uniquePaths(int m, int n) {

        int dp[][] = new int[m][n];
        dp[0][0] = 1;
        for (int i = 1; i < m; i++) {
            dp[i][0] = 1;
        }
        for (int j = 1; j < n; j++) {
            dp[0][j] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j]+dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
```



## [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)

难度中等

在一个由 `'0'` 和 `'1'` 组成的二维矩阵内，找到只包含 `'1'` 的最大正方形，并返回其面积。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)

```text
输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
输出：4
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)

```text
输入：matrix = [["0","1"],["1","0"]]
输出：1
```

**示例 3：**

```text
输入：matrix = [["0"]]
输出：0
```

 

**提示：**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 300`
- `matrix[i][j]` 为 `'0'` 或 `'1'`

------



```java
public static int maximalSquare(char[][] matrix) {
    //定义dp[i][j]表示以i行j列为右下角的且值全为1的正方形
    //则当matrix[i][j]=0时,dp[i][j]=0
    //则当matrix[i][j]=1时
    //dp[i][j] = max(1,min(dp[i-1][j,dp[i][j-1],dp[i-1][j-1])])
        int rows = matrix.length;
        int columns = matrix[0].length;
        int[][] dp = new int[rows][columns];
        int maxSide = 0;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                if (matrix[i][j] == '1') {
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1;
                    } else {
                        dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i - 1][j - 1]), dp[i][j - 1]) + 1;
                    }
                }
                maxSide = Math.max(maxSide, dp[i][j]);
            }
        }
        return maxSide * maxSide;
```



## [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

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

------

思路：动态规划

```java
    public static String longestPalindrome0(String s) {
        int longest = 0;
        int length = s.length();
        int start = 0;
        int end = 0;
        //dp[i][j]表示下标从i到j(i<=j)的子串是回文串的长度
        int[][] dp = new int[length][length];
        for (int j = 0; j < length; j++) {
            for (int i = j; i >= 0; i--) {
                if (i == j) {
                    dp[i][j] = 1;
                } else if (i == j - 1) {
                    dp[i][j] = s.charAt(i) == s.charAt(j) ? 2 : 0;
                } else {
                    if (dp[i + 1][j - 1] == 0) {
                        dp[i][j] = 0;
                    } else {
                        dp[i][j] = s.charAt(i) == s.charAt(j) ? 2 + dp[i + 1][j - 1] : 0;
                    }
                }
                if (dp[i][j] > longest) {
                    longest = dp[i][j];
                    start = i;
                    end = j;
                }
            }
        }
        return s.substring(start, end + 1);
    }
```

缺点：速度慢，因为需要算出所有情况

中心遍历法

```java
        int length = s.length();
        int longest = 0;
        int longestStart = 0;
        for (int i = 0; i < length; i++) {
            for (int j = 0; j < 2; j++) {
                int start = i;
                int end = i + j;
                while (start >= 0 && end < length && s.charAt(start) == s.charAt(end)) {
                    start--;
                    end++;
                }
                ////注意这里start--后end++后，需要从+1到-1还原长度
                int currentLongest = end - start - 1;
                if (currentLongest > longest) {
                    longest = currentLongest;
                    longestStart = start + 1;
                }
            }
        }
        return s.substring(longestStart, longestStart + longest);
```



## [1143. 最长公共子序列](/https://leetcode.cn/problems/longest-common-subsequence/description/?envType=study-plan-v2&envId=top-100-liked)

给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列** ，返回 `0` 。

一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

- 例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。

两个字符串的 **公共子序列** 是这两个字符串所共同拥有的子序列。

 

**示例 1：**

```
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。
```

**示例 2：**

```
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc" ，它的长度为 3 。
```

**示例 3：**

```
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0 。
```

 

**提示：**

- `1 <= text1.length, text2.length <= 1000`
- `text1` 和 `text2` 仅由小写英文字符组成。



---

思路，依然按照行列组成二维动态规划

```
    public int longestCommonSubsequence(String text1, String text2) {
        int rows = text1.length();
        int columns = text2.length();
        int[][] dp = new int[rows + 1][columns + 1];
        for (int i = 1; i < rows + 1; i++) {
            int char1 = text1.charAt(i - 1);
            for (int j = 1; j < columns + 1; j++) {
                char char2 = text2.charAt(j - 1);
                if (char1 == char2) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }
        return dp[rows][columns];
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



## [287. 寻找重复数](/https://leetcode.cn/problems/find-the-duplicate-number/description/?envType=study-plan-v2&envId=top-100-liked)

给定一个包含 `n + 1` 个整数的数组 `nums` ，其数字都在 `[1, n]` 范围内（包括 `1` 和 `n`），可知至少存在一个重复的整数。

假设 `nums` 只有 **一个重复的整数** ，返回 **这个重复的数** 。

你设计的解决方案必须 **不修改** 数组 `nums` 且只用常量级 `O(1)` 的额外空间。

 

**示例 1：**

```
输入：nums = [1,3,4,2,2]
输出：2
```

**示例 2：**

```
输入：nums = [3,1,3,4,2]
输出：3
```

 

**提示：**

- `1 <= n <= 105`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- `nums` 中 **只有一个整数** 出现 **两次或多次** ，其余整数均只出现 **一次**

 

**进阶：**

- 如何证明 `nums` 中至少存在一个重复的数字?
- 你可以设计一个线性级时间复杂度 `O(n)` 的解决方案吗



---

思路：长度为n数组放了0到n-1的数，可以想象一个线性的链表

如果放了0到n的n+1个数，则链表必然有环，重复的那个数就是环的入口点，算法同环形链表二



## [75. 颜色分类][/https://leetcode.cn/problems/sort-colors/description/?envType=study-plan-v2&envId=top-100-liked]

中等



给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums` ，**原地**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色。



必须在不使用库内置的 sort 函数的情况下解决这个问题。

 

**示例 1：**

```
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]
```

**示例 2：**

```
输入：nums = [2,0,1]
输出：[0,1,2]
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` 为 `0`、`1` 或 `2`

 

**进阶：**

- 你能想出一个仅使用常数空间的一趟扫描算法吗？



---

思路：双指针分别指向结尾0的下一个left和开头2的前一个right ，遍历当是2时，和right交换。。。

```
    public void sortColors(int[] nums) {
        int length = nums.length;
        int left = 0;
        int right = length - 1;
        for (int i = 0; i <= right; i++) {
            while (nums[i] == 2 && i <= right) {
                nums[i] = nums[right];
                nums[right] = 2;
                right--;
            }
            if (nums[i] == 0) {
                nums[i] = nums[left];
                nums[left] = 0;
                left++;
            }
        }
    }
```



## [31. 下一个排列](/https://leetcode.cn/problems/next-permutation/description/?envType=study-plan-v2&envId=top-100-liked)

中等

整数数组的一个 **排列**  就是将其所有成员以序列或线性顺序排列。

- 例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]` 。

整数数组的 **下一个排列** 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 **下一个排列** 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

- 例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。
- 类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。
- 而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。

给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须 **原地** 修改，只允许使用额外常数空间。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：nums = [3,2,1]
输出：[1,2,3]
```

**示例 3：**

```
输入：nums = [1,1,5]
输出：[1,5,1]
```

 

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

---

思路： abcd从后向前遍历，

d

c比d小，则c放在d后，返回abdc，否则c>=d,abdc,继续遍历，

b比dc中的一个数小，则找到第一个比b大的数，与之交换，返回，否则adcb继续遍历

a同b

一次通过！其实是个数学问题

```
    /**
     * @param nums 31. 下一个排列
     */
    public static void nextPermutation(int[] nums) {
        int length = nums.length;
        int maxNum = nums[length-1];
        for (int i = length-1; i >=0 ; i--) {
            //此时找到了下一个排列
            if (nums[i]<maxNum){
                //将后面的反转
                reverse(nums,i+1,length-1);
                //找到第一个比当前数大的，交换位置
                for (int j = i+1; j < length; j++) {
                    if (nums[j]>nums[i]){
                        int temp = nums[j];
                        nums[j] =nums[i];
                        nums[i] = temp;
                        return;
                    }
                }

            }else {
                maxNum = nums[i];

            }
        }
        //此时已经是最大的排列，只需反转即可
        reverse(nums,0,length-1);
    }
    private static void reverse(int[] nums,int start,int end){
        while (start<end){
            int temp = nums[start];
            nums[start] =nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
```

官方做法更简洁：

```
    public static void nextPermutation2(int[] nums) {
        //首先从最后一个开始找，找到第一个非降序排列的数
        //即假设从i+1到j都是按降序排列的，只要nums[i]不比nums[i+1]大。则找到了
        int i = nums.length-2;
        while (i>=0&&nums[i]>=nums[i+1]){
            i--;
        }
        //到没找到时i=-1，找到时i那个数是
        if (i>-1){
            int j = nums.length-1;
            while (nums[j]>=nums[i]){
                j--;
            }
            int temp = nums[j];
            nums[j] =nums[i];
            nums[i] = temp;
        }
        reverse(nums,i+1,nums.length-1);
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





