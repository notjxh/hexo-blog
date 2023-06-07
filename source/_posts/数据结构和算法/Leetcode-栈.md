---
title: Leetcode-栈
date: 2022-02-23 11:13:12
tags: Leetcode-栈
description:
categories: Leetcode
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

