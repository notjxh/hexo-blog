---
title: Leetcode-数组
date: 2022-02-23 09:59:45
tags: Leetcode-数组
description:
categories: Leetcode
---
## [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)
难度简单

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
你可以按任意顺序返回答案。
 
示例 1：
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

示例 2：
输入：nums = [3,2,4], target = 6
输出：[1,2]

示例 3：
输入：nums = [3,3], target = 6
输出：[0,1]

提示：
2 <= nums.length <= 103
-109 <= nums[i] <= 109
-109 <= target <= 109
只会存在一个有效答案

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

将时间复杂度从O(n²)降到O(n)：通过hash查找
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

##  [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)


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

---


重点是把这道题转换成动态规划的思想来解决：
https://leetcode-cn.com/problems/maximum-subarray/solution/xiang-xi-jie-du-dong-tai-gui-hua-de-shi-xian-yi-li/

对nums[i]遍历，当遍历到第i个元素时，计算以第i个元素为结尾的子列的最大和，我们记为sumNow，我们可以知道sumNow由以第i-1个元素结尾的子列
（记最大和为sumPro）和nums[i]组成，比较每一次的SumMax和sumNow


最大和连续子列一定是以某个节点为结束点的连续子列
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
        preSum = Math.max(preSum,preSum+num);
        maxSum = Math.max(maxSum,preSum);
    }
    return maxSum;
}
```


还有一种 todo分治法：


## [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)


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

---

思路：
第i天的最大利润=第i天的价格-前i-1天的最低价格
比较第i-1天的最大利润，取最大值

```java
    /**
     * @param prices
     * @return 121. 买卖股票的最佳时机
     */
    public int maxProfit(int[] prices){
        if (prices.length==1){
            return 0;
        }
        int maxProfit = 0;
        int minPrice = prices[0];
        for (int i = 1; i < prices.length; i++) {
            if (prices[i]<minPrice){
                minPrice = prices[i];
            }
            if (prices[i]-minPrice>maxProfit){
                maxProfit = prices[i]-minPrice;
            }
        }
        return maxProfit;
    }
}
```

执行用时：2 ms, 在所有 Java 提交中击败了85.02%的用户
内存消耗：51.4 MB, 在所有 Java 提交中击败了42.22%的用户

优化：但是运行更慢了点 3 ms,

```java
public static int maxProfit2(int[] prices){
    //优化
    if (prices==null||prices.length<=1){
        return 0;
    }
    int maxProfit = 0;
    int minPrice = prices[0];
    for (int i = 1; i < prices.length; i++) {
        //优化
        minPrice = Math.min(minPrice,prices[i]);
        maxProfit = Math.max(prices[i]-minPrice,maxProfit);
    }
    return maxProfit;
}
```

定义数组可以不初始化，可以赋值为null



## [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

难度简单


给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

示例 1：

输入：[3,2,3] 输出：3

示例 2：

输入：[2,2,1,1,1,2,2] 输出：2 

**进阶：**

- 尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题。

---

思路：

有一个摩尔投票法

形象解释：

诸侯乱战，其中有一方诸侯A的人数超过总人数的一半，这样不是相同一方的1v1抵消，则最后剩下的一定是人数最多的那一方诸侯，最差的是所有其他诸侯联合打A，
但是还是A会赢


```java
/**
 * @param nums
 * @return 169. 多数元素
 */
public static int majorityElement(int[] nums) {
    int maj = nums[0];
    int count = 0;
    //错误：i=0 不是1，因为第一个也要比对，相同要算上1
    for (int i = 0; i < nums.length; i++) {
        if (maj==nums[i]){
            count++;
        }else {
            count--;
            if (count==0){
                //因为多数一定存在，则最多抵消到倒数第二位数，即i最大为n-2，i=1不会越界
                maj=nums[i+1];
            }
        }
    }
    return maj;
}
```

执行用时：1 ms, 在所有 Java 提交中击败了99.99%的用户

内存消耗：41.6 MB, 在所有 Java 提交中击败了77.32%的用户



## [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

难度简单


给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:

输入: [0,1,0,3,12] 输出: [1,3,12,0,0]

说明:

1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。

---

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

