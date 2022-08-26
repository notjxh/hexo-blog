---
title: JVM
date: 2022-06-16 09:18:43
tags:
description:
categories:
---

## 什么是JVM

定义：Java Virtual Machine：java字节码的运行环境

好处：

* 一次编写，到处运行
* 自动内存管理，垃圾回收功能
* 数组下标越界检查？
* 多态？

比较：JVM、JRE、JDK

![1.png](1.png)



## 学习路线

![2.png](2.png)



jvm内存结构

gc垃圾回收

字节码文件

类加载器

执行引擎



## 内存结构

### 程序计数器

#### 定义

Program Counter Register程序计数器（寄存器）

#### 作用

java源代码-编译成二进制字节码（jvm指令，每个系统都一样）-交给解释器-翻译成机器码-交给cpu执行

程序计数器：就是来记住下一条jvm指令的执行地址

#### 特点

线程私有的:每个线程都有自己的自己的程序计数器

PCR是jvm中唯一不存在内存溢出的(jvm规范规定)

### 虚拟机栈

#### 定义

JVM Stacks :java 虚拟机栈

* 每个线程 运行时需要的内存，称为虚拟机栈

* 每个栈由多个栈帧（Frame)组成，对应着每次方法调用时所占用的内存（参数、局部变量、返回地址）
* 每个线程只能有一个活动栈帧，对应着当前正在执行的的那个方法

#### 代码演示

idea中的frams内看到当前线程中的栈，其中最上面的就是活动栈帧

![2_1.png](2_1.png)



### 本地方法栈

带有native关键字的方法，如Object类里的hashCode()、clone()、notify()、wait()，通过c、c++等语言调用底层系统。

### 堆

#### 定义

Heap 堆：通过new 关键字，创建的对象都会使用堆内存

#### 特点

* 线程共享，因此堆中对象都需要考虑线程安全问题

* 有垃圾回收机制

#### 堆内存溢出

模拟堆内存溢出：过多的对象被使用，不能被回收导致堆内存溢出。

```
    public static void testHeapOutOfMemory(){
        int i =0;
        String str = "hello";
        List<String> list = new ArrayList<>();
        try {
            while (true){
                list.add(str);
                str = str + str;
                i++;
            }
        } catch (Throwable e) {
            System.out.println(i);
            e.printStackTrace();
        }
    }
```

> 25次后报错：java.lang.OutOfMemoryError: Java heap space

设置堆内存大小 `-Xmx10m` `-Xmx2g`

#### 堆内存诊断

1. jps工具

   命令行输入：jps 查看当前运行了哪些java进程

2. jmap工具

   命令行输入： jmap -heap 进程号 查看堆内存占用情况

3. jconsole工具

   命令行输入：jconsole 图形化连续展示堆内存占用情况

4. jvisualvm工具

   案例：经过多次gc,内存占用还是很高

   命令行输入：jvisualvm：将某一时刻的内存和对象状态转存出来，可以看到students占内存200m

```
    public  void testJvisualvm() throws InterruptedException{
        List<Student> students = new ArrayList<>();
        for (int i = 0; i < 200; i++) {
            students.add(new Student());
//            Student student = new Student();
        }
        Thread.sleep(1000000000L);
    }

    class Student {
        private byte[] big = new byte[1024*1024];
    }
```







### 方法区

#### 定义

[官方定义](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.5.4)



包括类的信息，类加载器，常量池（其中有StringTable）



方法区是一种规范，

jdk1.8以前实现的方式叫永久代，放在堆内存里

jdk1.8以后实现的方式叫元空间，放在本地内存。

![3.png](3.png)



#### 方法区内存溢出

演示：

```
import jdk.internal.org.objectweb.asm.ClassWriter;
import jdk.internal.org.objectweb.asm.Opcodes;

/**
 * 示元空间内存溢出 java.lang.OutOfMemoryError: Metaspace
 *  * -XX:MaxMetaspaceSize=8m
 */
public class MethodAreaDemo extends ClassLoader {
    public static void main(String[] args) {
        int j = 0;
        try {
            MethodAreaDemo test = new MethodAreaDemo();
            for (int i = 0; i < 100000; i++, j++) {
                // ClassWriter 作用是生成类的二进制字节码
                ClassWriter cw = new ClassWriter(0);
                // 版本号， public， 类名, 包名, 父类， 接口
                cw.visit(Opcodes.V1_8, Opcodes.ACC_PUBLIC, "Class" + i, null, "java/lang/Object", null);
                // 返回 byte[]
                byte[] code = cw.toByteArray();
                // 执行了类的加载
                test.defineClass("Class" + i, code, 0, code.length); // Class 对象
            }
        } finally {
            System.out.println(j);
        }
    }
}

```

设置方法区大小：

`-XX:MaxMetaspaceSize=8m`时：

报错：MaxMetaspaceSize is too small.

`-XX:MaxMetaspaceSize=10m`时：

报错：Exception in thread "main" java.lang.OutOfMemoryError: Compressed class space

设为30m时才出现：

JDK1.8后：

> Exception in thread "main" java.lang.OutOfMemoryError: Metaspace



JDK1.8以前：

> java.lang.OutOfMemoryError: PermGen space



#### 运行时常量池

常量池：

编译一个helloworldjava程序，对生成的class文件，执行 javap -v HelloWorld.class

![4.png](4.png)

常量池：记录了很多指令#01 # 02，编译后的程序根据常量池查找具体指令的含义（类名、方法名、参数类型，字面量等信息）

运行时常量池：常量池是.class文件中的，当类被加载，它的常量池信息就会被放到运行时常量池里，并把里面的符号#01#02变成真实地址



####  StringTable面试题



