---
title: Leetcode-树
date: 2022-02-23 11:13:44
tags: Leetcode-树
description:
categories: Leetcode
---
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

---

思路：题解

迭代：

```java
LinkedList<TreeNode> queue = new LinkedList<>();
queue.add(root.left);
queue.add(root.right);
while (queue.size()>0){
    TreeNode left = queue.removeFirst();
    TreeNode right = queue.removeFirst();
    if (left==null&&right==null){
        continue;
    }
    if (left==null||right==null||left.val!=right.val){
        return false;
    }
    queue.add(left.left);
    queue.add(right.right);
    queue.add(left.right);
    queue.add(right.left);
}
return true;
```

执行用时：1 ms, 在所有 Java 提交中击败了27.72%的用户

内存消耗：38.1 MB, 在所有 Java 提交中击败了5.02%的用户

todo:效率，递归

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
```

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

---

思路：没有，看题解

```java
/**
 * @param root
 * @return 104. 二叉树的最大深度
 * 递归解法：DFS 深度优先算法？
 */
public int maxDepth(TreeNode root) {
    if (root==null){
        return 0;
    }
    return Math.max(maxDepth(root.left),maxDepth(root.right))+1;
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：38.4 MB, 在所有 Java 提交中击败了66.59%的用户

*todo 广度优先算法*


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

---

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

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：36.3 MB, 在所有 Java 提交中击败了5.19%的用户


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

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：36.1 MB, 在所有 Java 提交中击败了7.56%的用户


## [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)


难度简单

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

**示例 :**
给定二叉树

```text
          1
         / \
        2   3
       / \     
      4   5    
```

返回 **3**, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。


**注意：**两结点之间的路径长度是以它们之间边的数目表示。

---

思路：没有 看题解

以每个节点为根节点的左右子树的最大深度-1之和

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
    return ans ;
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

## [617. 合并二叉树-TODO](https://leetcode-cn.com/problems/merge-two-binary-trees/)

难度简单

给你两棵二叉树： `root1` 和 `root2` 。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，**不为** null 的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

**注意:** 合并过程必须从两个树的根节点开始。

 
**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/05/merge.jpg)

输入：root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
输出：[3,4,5,5,4,null,7]

**示例 2：**

输入：root1 = [1], root2 = [1,2]
输出：[2,2]

**提示：**

- 两棵树中的节点数目在范围 `[0, 2000]` 内
- `-104 <= Node.val <= 104`

---



## [94. 二叉树的中序遍历-TODO](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

难度简单

给定一个二叉树的根节点 `root` ，返回它的 **中序** 遍历。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```text
输入：root = [1,null,2,3]
输出：[1,3,2]
```

**示例 2：**

```text
输入：root = []
输出：[]
```

**示例 3：**

```text
输入：root = [1]
输出：[1]
```

**示例 4：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_5.jpg)

```text
输入：root = [1,2]
输出：[2,1]
```

**示例 5：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_4.jpg)

```text
输入：root = [1,null,2]
输出：[1,2]
```

 

**提示：**

- 树中节点数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

 

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？

---



