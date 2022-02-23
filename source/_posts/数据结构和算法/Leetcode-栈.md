---
title: Leetcode-栈
date: 2022-02-23 11:13:12
tags: Leetcode-栈
description:
categories: Leetcode
---


## [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)



难度简单


给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

 
示例 1：

输入：s = "()" 输出：true 

示例 2：

输入：s = "()[]{}" 输出：true 

示例 3：

输入：s = "(]" 输出：false 

示例 4：

输入：s = "([)]" 输出：false 

示例 5：

输入：s = "{[]}" 输出：true


**提示：**

- 1 <= s.length <= 104
- s 仅由括号 '()[]{}' 组成

---

思路：

先进的元素要先出去，即栈的用法：引申Stack类

```java
public static boolean isValid(String s) {
    Stack<String> stringStack = new Stack<>();
    if (s.length()% 2 != 0) {
        return false;
    }
    String[] strings = s.split("");
    for (String string : strings) {

        if (string.equals("(") || string.equals("{") || string.equals("[")) {
            stringStack.push(string);
        }
        else if (string.equals(")")) {
            if (stringStack.empty() || !stringStack.pop().equals("(")) {
                return false;
            }
        }
        else if (string.equals("}")) {
            if (stringStack.empty() || !stringStack.pop().equals("{")) {
                return false;
            }
        }
        else if (string.equals("]")) {
            if (stringStack.empty() || !stringStack.pop().equals("[")) {
                return false;
            }
        }
    }
    return stringStack.empty();
}
```

缺点是运行较慢：以下这种取巧解法运行也慢

```java
public static boolean isValid2(String s) {
    int length = s.length()/2;
    for (int i = 0; i <length; i++) {
        s = s.replace("{}","").replace("()","").replace("[]","");
    }
    return s.length()==0;
}
```

执行用时：1 ms, 在所有 Java 提交中击败了99.17%的用户

内存消耗：36.5 MB, 在所有 Java 提交中击败了71.30%的用户

```java
public boolean isValid3(String s) {
    Stack<Character> stringStack = new Stack<>();
        if (s.length()% 2 != 0) {
            return false;
        }
        char[] chars = s.toCharArray();
        for (char aChar : chars) {

            if (aChar=='(') {
                stringStack.push(')');
            }
            else if (aChar=='{') {
                stringStack.push('}');
            }
            else if (aChar=='[') {
                stringStack.push(']');
            }else {
                if (stringStack.empty()){
                    return false;
                }else {
                    if (stringStack.pop()!=aChar){
                        return false;
                    }
                }
            }
        }
        return stringStack.empty();
}

```


## [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)



难度简单


设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

- push(x) —— 将元素 x 推入栈中。
- pop() —— 删除栈顶的元素。
- top() —— 获取栈顶元素。
- getMin() —— 检索栈中的最小元素。

 
示例:
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


**提示：**

- pop、top 和 getMin 操作总是在 **非空栈** 上调用。

---

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

