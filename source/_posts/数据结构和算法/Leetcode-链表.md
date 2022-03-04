---
title: Leetcode-链表
date: 2022-02-23 11:13:34
tags: Leetcode-链表
description:
categories: Leetcode
---

```java
    class ListNode {
        int val;
        ListNode next;

        ListNode() {
        }

        ListNode(int val) {
            this.val = val;
        }

        ListNode(int val, ListNode next) {
            this.val = val;
            this.next = next;
        }
    }
```

##  [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

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

---

思路：

1.怎么用java表示链表？**ListDemo**

https://blog.csdn.net/weixin_36605200/article/details/88804537

引申 访问修饰符https://blog.csdn.net/qq_43166888/article/details/103339537

2.分别遍历两个链表的值，记录



题解：迭代法：注意随意定义一个头结点，用头结点的下一个节点开始

```java
public ListNode mergeTwoLists3(ListNode l1, ListNode l2) {
    ListNode newListNode = new ListNode(-1);
    ListNode prev = newListNode;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            prev.next = l1;
            l1 = l1.next;
        } else {
            prev.next = l2;
            l2 = l2.next;
        }
        prev = prev.next;
    }
    prev.next = l1 == null ? l2 : l1;
    return newListNode.next;
}
```



递归法：待理解

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



## [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

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

---

思路：遍历每个节点，有相同的则是有环，问题是怎么判断是否相同？

引申HashSet的底层结构



```java
public boolean hasCycle(ListNode head) {
    Set<ListNode> set = new HashSet<>();
    while (head!=null){
        boolean add = set.add(head);
        if (!add){
            return true;
        }
        head = head.next;
    }
    return false;

}
```



执行用时：5 ms, 在所有 Java 提交中击败了14.25%的用户

内存消耗：39.2 MB, 在所有 Java 提交中击败了53.97%的用户





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



执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：39.6 MB, 在所有 Java 提交中击败了26.74%的用户





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

---



思路：重点是仅用 O(*1*) 内存

如果不是，看题解：



方法一：暴力解法，遍历A，一个个对比B看是否有相同

时间复杂度：O(mn) 空度：O(1)



方法二：利用哈希表的查找时间复杂度是O(1)，

将A的遍历结果放在哈希表中，再放入B的

时度：O(m+n) 空度：n



方法三：题解方法，双指针法：



指针1从头遍历A，遍历完了从从遍历B，指针2从头遍历B，遍历完了从从遍历A

如果两两链表相交，则两个指针会同时到达相交的点

原因(A+B-相交之后的公共部分)长度=（B+A-相交之后的公共部分）长度



哈希表可以将查找的时度从n降为1，但是一般情况下需要空度n



```java
ListNode preA = headA;
ListNode preB = headB;
//当不想交都到末尾时，preA==preB=null
while (preA!=preB){
    //解题时将preA.next == null作为判断
    if (preA == null) {
        preA = headB;
    }else {
        preA = preA.next;
    }
    if (preB == null) {
        preB = headA;
    } else {
        preB=preB.next;
    }
}
return preA;
```





## [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)



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

---

迭代法：注意这里需要三个指针，temp  作为临时指针

```java
public ListNode reverseList(ListNode head) {
        if (head==null){
            return null;
        }
        // 这里将当前节点定义为head，再定义一个前节点更好prev=null;
        ListNode curr = head;
        ListNode next = curr.next;
        head.next=null;
        while (next!=null){
            ListNode temp  = next.next;
            next.next = curr;
            curr = next;
            next = temp;
        }
        return curr;
    }
```





题解递归法todo：

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode p = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return p;
}

```





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

---



思路：什么是回文链表

类比字符串：回文字符串是指从字符串中间开始，两遍的字符相对于比如abba aca

问题是链表是单链表，只能从头开始遍历



看题解：快慢指针的解法，快指针2到末尾时，慢指针1到中间，分别移动指针比较，空间复杂度1



误区：不需要考虑奇数个还是偶数个，都从slow的下一个节点开始反转



反转是否会导致原链表修改？？



```java
/**
 * @param head
 * @return 234. 回文链表
 */
public boolean isPalindrome(ListNode head) {
    ListNode pre = new ListNode();
    pre.next = head;
    ListNode fast = pre;
    ListNode slow = pre;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    //此时无论是有技术还是偶数个 slow.next就是后半段的起点
    
    ListNode reverse = reverseList2(slow.next);

    while (reverse != null) {
        if (reverse != head) {
            return false;
        }
        reverse = reverse.next;
        head = head.next;
    }

    return true;
}
```

## [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

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

---

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

执行用时：2 ms, 在所有 Java 提交中击败了100.00%的用户
内存消耗：38.7 MB, 在所有 Java 提交中击败了81.80%的用户



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

---

思路：
第一遍扫描出链表的长度sz，第二遍扫描第sz-n+1个结点，就是倒数第n个结点
进阶：一次扫描要找到倒数第n个节点，需要用到快慢`双指针`

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

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户
内存消耗：36.2 MB, 在所有 Java 提交中击败了88.52%的用户





## [142. 环形链表 II-TODO](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

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

---

思路：怎么判断入环的第一个结点？快慢指针相交可以判断有环，在此基础上怎么获取第一个相交结点？
用set存储，第一个重复的就是；







## [148. 排序链表-TODO](https://leetcode-cn.com/problems/sort-list/)

难度中等

给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。



 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg)

```text
输入：head = [4,2,1,3]
输出：[1,2,3,4]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg)

```text
输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]
```

**示例 3：**

```text
输入：head = []
输出：[]
```

 

**提示：**

- 链表中节点的数目在范围 `[0, 5 * 104]` 内
- `-105 <= Node.val <= 105`

 

**进阶：**你可以在 `O(n log n)` 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

---

思路：

新建一个链表，依次遍历原链表，插入到新链表的合适位置

时复：n*n  空复：n

时间不满足