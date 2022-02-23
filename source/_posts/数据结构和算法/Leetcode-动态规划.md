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

## [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

怎么定位到其他文章的某个段落？

{% post_link Leetcode-数组.md %}

##  [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

## [338. 比特位计数-TODO](https://leetcode-cn.com/problems/counting-bits/)



## [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

难度中等

给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

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

---



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

执行用时：2 ms, 在所有 Java 提交中击败了96.49%的用户

内存消耗：41.2 MB, 在所有 Java 提交中击败了43.62%的用户





##  [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

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

---

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

---

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





##  [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)

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

---

//注意1： char类型的'0''1'转换成int类型

dp[i][0] = matrix[i][0]-'0';

//注意2： 这里也要取出最大值

```java
public static int maximalSquare(char[][] matrix) {
    //定义dp[i][j]表示以i行j列为右下角的且值全为1的正方形
    //则当matrix[i][j]=0时,dp[i][j]=0
    //则当matrix[i][j]=1时
    //dp[i][j] = max(1,min(dp[i-1][j,dp[i][j-1],dp[i-1][j-1])])
    //初始化第一行，第一列
    int rows = matrix.length;
    int columns = matrix[0].length;
    int maxResult = 0;
    int dp[][] = new int[rows][columns];
    for(int i =0;i<rows;i++){
        //注意： char类型的'0''1'转换成int类型
        dp[i][0] = matrix[i][0]-'0';
        //注意： 这里也要取出最大值
        maxResult = Math.max(maxResult,dp[i][0]);
    }
    for(int j =0;j<columns;j++){
        dp[0][j] = matrix[0][j]-'0';
        maxResult = Math.max(maxResult,dp[0][j]);
    }
    for(int i =1;i<rows;i++){
        for(int j =1;j<columns;j++){
            if(matrix[i][j]!='1')
            {
                dp[i][j] = 0;
            }else{
                int min = Math.min(dp[i-1][j],dp[i][j-1]);
                min =  Math.min(min,dp[i-1][j-1]);
                dp[i][j] = min+1;
            }
            maxResult = Math.max(maxResult,dp[i][j]);
        }
    }
    return maxResult*maxResult;
}
```



执行用时：6 ms, 在所有 Java 提交中击败了57.33%的用户

内存消耗：49.5 MB, 在所有 Java 提交中击败了19.96%的用户



简化：可以只处理非0的情况 因为dp[i][j]的默认值就是0

```java
public static int maximalSquare2(char[][] matrix) {

    int rows = matrix.length;
    int columns = matrix[0].length;
    int maxResult = 0;
    int dp[][] = new int[rows][columns];

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
            if (matrix[i][j] == '1') {
                if (i == 0 || j == 0) {
                    dp[i][j] = 1;
                } else {
                    int min = Math.min(dp[i - 1][j], dp[i][j - 1]);
                    min = Math.min(min, dp[i - 1][j - 1]);
                    dp[i][j] = min + 1;
                }
            }
            maxResult = Math.max(maxResult, dp[i][j]);
        }
    }
    return maxResult * maxResult;
}
```



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







##  [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

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



---

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

