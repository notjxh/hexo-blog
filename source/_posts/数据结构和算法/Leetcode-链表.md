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



##  [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

难度简单

给你一个链表的头节点        

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg)

```
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]
```

**示例 2：**

```
输入：head = [], val = 1
输出：[]
```

**示例 3：**

```
输入：head = [7,7,7,7], val = 7
输出：[]
```

 

**提示：**

- 列表中的节点数目在范围 `[0, 104]` 内
- `1 <= Node.val <= 50`
- `0 <= val <= 50`

---

```
    public ListNode removeElements(ListNode head, int val) {
       if(head==null){
           return head;
       }
       head.next = removeElements(head.next,val);
       return head.val==val?head.next:head;
    }
```







迭代法：注意这里需要三个指针，temp  作为临时指针

```java
public ListNode reverseList(ListNode head) {
        
        ListNode pre = null;
        ListNode cur = head;
        while (cur!=null){
            ListNode next  = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return cur;
    }
```



递归法TODO：继续理解

head.next.next = head;将head的下一个节点的的下一个节点指向自己

将head的指向清空

```java
    public ListNode reverseList(ListNode head) {
        if(head==null || head.next==null){
            return head;
        }
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }

```



















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