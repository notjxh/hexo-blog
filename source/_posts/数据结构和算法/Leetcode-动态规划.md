---
title: Leetcode-动态规划
date: 2022-02-23 13:51:44
tags: Leetcode-动态规划
description:
categories: Leetcode
---













## [309. 最佳买卖股票时机含冷冻期-TODO](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

难度中等

给定一个整数数组`prices`，其中第  `prices[i]` 表示第 `*i*` 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

- 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 

**示例 1:**

```text
输入: prices = [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

**示例 2:**

```text
输入: prices = [1]
输出: 0
```

 

**提示：**

- `1 <= prices.length <= 5000`
- `0 <= prices[i] <= 1000`

---











##  [494. 目标和-TODO](https://leetcode-cn.com/problems/target-sum/)

难度中等

给你一个整数数组 `nums` 和一个整数 `target` 。

向数组中的每个整数前添加 `'+'` 或 `'-'` ，然后串联起所有整数，可以构造一个 **表达式** ：

- 例如，`nums = [2, 1]` ，可以在 `2` 之前添加 `'+'` ，在 `1` 之前添加 `'-'` ，然后串联起来得到表达式 `"+2-1"` 。

返回可以通过上述方法构造的、运算结果等于 `target` 的不同 **表达式** 的数目。

 

**示例 1：**

```text
输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

**示例 2：**

```text
输入：nums = [1], target = 1
输出：1
```

 

**提示：**

- `1 <= nums.length <= 20`
- `0 <= nums[i] <= 1000`
- `0 <= sum(nums[i]) <= 1000`
- `-1000 <= target <= 1000`

---

回溯





## [406. 根据身高重建队列-TODO](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

难度中等

假设有打乱顺序的一群人站成一个队列，数组 `people` 表示队列中一些人的属性（不一定按顺序）。每个 `people[i] = [hi, ki]` 表示第 `i` 个人的身高为 `hi` ，前面 **正好** 有 `ki` 个身高大于或等于 `hi` 的人。

请你重新构造并返回输入数组 `people` 所表示的队列。返回的队列应该格式化为数组 `queue` ，其中 `queue[j] = [hj, kj]` 是队列中第 `j` 个人的属性（`queue[0]` 是排在队列前面的人）。



**示例 1：**

```text
输入：people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
输出：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
解释：
编号为 0 的人身高为 5 ，没有身高更高或者相同的人排在他前面。
编号为 1 的人身高为 7 ，没有身高更高或者相同的人排在他前面。
编号为 2 的人身高为 5 ，有 2 个身高更高或者相同的人排在他前面，即编号为 0 和 1 的人。
编号为 3 的人身高为 6 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
编号为 4 的人身高为 4 ，有 4 个身高更高或者相同的人排在他前面，即编号为 0、1、2、3 的人。
编号为 5 的人身高为 7 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
因此 [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] 是重新构造后的队列。
```

**示例 2：**

```text
输入：people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
输出：[[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
```

 

**提示：**

- `1 <= people.length <= 2000`
- `0 <= hi <= 106`
- `0 <= ki < people.length`
- 题目数据确保队列可以被重建

---

思路:看了n遍才看懂题目

原数组people[I]是被打乱后的数组，要求原来的满足格式的数组queue；





##  [78. 子集](https://leetcode-cn.com/problems/subsets/)

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

---

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

