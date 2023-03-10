---
title: java核心技术一
date: 2023-03-09 10:27:31
tags:
description:
categories:
---

# 第4章 对象与类

## 4.1 面向对象概述

面向对象程序设计（简称OOP）是当今主流的程序设计范型，它已经取代了20世纪70年代的“结构化”过程化程序设计开发技术。

### 4.1.1 类

类（class）是构造对象的模板或蓝图。我们可以将类想象成制作小甜饼的切割机，将对象想象为小甜饼。由类构造（construct）对象的过程称为创建类的实例（instance）。

类（class）是构造对象的模板或蓝图。我们可以将类想象成制作小甜饼的切割机，将对象想象为小甜饼。由类构造（construct）对象的过程称为创建类的实例（instance）。

对象中的数据称为实例域（instance field），操纵数据的过程称为方法（method）。对于每个特定的类实例（对象）都有一组特定的实例域值。这些值的集合就是这个对象的当前状态（state）。

通过扩展一个类来建立另外一个类的过程称为继承（inheritance）。

### 4.1.2 对象

* 对象的行为（behavior）——可以对对象施加哪些操作，或可以对对象施加哪些方法？
* 对象的状态（state）——当施加那些方法时，对象如何响应？
* 对象标识（identity）——如何辨别具有相同行为与状态的不同对象？

### 4.1.4 类之间的关系
依赖（dependence），即“uses-a”关系，。例如，Order类使用Account类是因为Order对象需要访问Account对象查看信用状态。

聚合（aggregation），即“has-a”关系。例如，一个Order对象包含一些Item对象。

继承（inheritance），即“is-a”关系。例如，Rush Order类由Order类继承而来。

## 4.2 使用预定义类
Math类，Date类和LocalDate类

### 4.2.1 对象与对象变量

要想使用对象，就必须首先构造对象，并指定其初始状态。然后，对对象应用方法。

在Java程序设计语言中，使用构造器（constructor）构造新实例。如new Date()。

通常，希望构造的对象可以多次使用，因此，需要将对象存放在一个变量中：Date birthday = new Date();

注意：一个对象变量实际并没有包含一个对象，而仅仅引用一个对象。如Date deadline;定义了一个对象变量deadline，它可以引用一个Date类型的变量，但是变量deadline不是一个对象，也没有引用对象，因此不能将Date方法应用到这个变量上。

Java中，任何一个对象变量的值都是对存储在另一个地方的一个对象的引用。new操作符的返回值也是一个引用。

Date deadline = new Date();的含义是：表达式new Date()构造了一个Date类型的对象，并且它的值是对新对象的引用。这个引用存储在对象变量deadline中。

局部变量不会自动地初始化为null，而必须通过调用new或将它们设置为null进行初始化。

### 4.2.2 Java类库中的LocalDate类

使用静态工厂方法来构造LocalDate对象：
> LocalDate.now();


### 4.2.3 更改器方法和访问器方法

只访问对象而不修改对象的方法有时称为访问器方法（accessor method）。例如，LocalDate.getYear和GregorianCalendar.get就是访问器方法。

GregorianCalendar.add方法是一个更改器方法（mutator method）。调用这个方法后，someDay对象的状态会改变。

todo：利用LocalDate类生成一个当前月的日历

## 4.3 用户自定义类

```
 * 职员类
 */
public class Employee {
//    private static int nextId;

    private String name;
    private double salary;
    private LocalDate hireDate;

    public Employee(String name, double salary, int year, int month, int day) {

        this.name = name;
        this.salary = salary;
        this.hireDate = LocalDate.of(year,month,day);
    }
    public Employee(String name ){
        this.name = name;
    }

//    public static int getNextId() {
//        return nextId;
//    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public LocalDate getHireDate() {
        return hireDate;
    }

    public void raiseSalary(double byPercent){
        double raise = salary * byPercent / 100;
        salary += raise;
    }

    @Override
    public String toString() {
        return "[Employee:"+name+"]";
    }
}
```
### 4.3.3 剖析Employee类

这个类的所有方法都被标记为public。关键字public意味着任何类的任何方法都可以调用这些方法
关键字private确保只有Employee类自身的方法能够访问这些实例域，而其他类的方法不能够读写这些域。

### 4.3.4 从构造器开始

构造器与其他的方法有一个重要的不同。构造器总是伴随着new操作符的执行被调用，而不能对一个已经存在的对象调用构造器来达到重新设置实例域的目的。构造器没有返回值

TODO 要记住所有的Java对象都是在堆中构造的

### 4.3.5 隐式参数与显式参数
对于方法
```
    public void raiseSalary(double byPercent){
        double raise = salary * byPercent / 100;
        salary += raise;
    }
```
当number007.raiseSalary(5);时，实际发生的是
```
        double raise = number007.salary * byPercent / 100;
        number007.salary += raise;
```
raiseSalary方法有两个参数。第一个参数称为隐式（implicit）参数，是出现在方法名前的Employee类对象。第二个参数位于方法名后面括号中的数值，这是一个显式（explicit）参数。（有些人把隐式参数称为方法调用的目标或接收者。）

在每一个方法中，关键字this表示隐式参数。
因此raiseSalary方法也可以写成：
```
        double raise = this.salary * byPercent / 100;
        this.salary += raise;
```
### 4.3.6 封装的优点
对于下面三个方法
```
    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public LocalDate getHireDate() {
        return hireDate;
    }
```
这些都是典型的访问器方法。由于它们只返回实例域值，因此又称为域访问器。

为何不将域name设为public，而通过一个public个getName()访问的好处：
1. 可以改变内部实现，除了该类的方法之外，不会影响其他代码。例如，如果将存储名字的域改为：String firstName；String lastName；那么getName方法可以改为放回 return firstName +lastName；对于该类的调用者来说没有改动，而只用改动该类内部。
2. 更改器方法可以执行错误检查，然而直接对域进行赋值将不会进行这些处理。例如，setSalary方法可以检查薪金是否小于0。

因此规范如下
● 一个私有的数据域；
● 一个公有的域访问器方法；
● 一个公有的域更改器方法。

注意：不要编写返回引用可变对象的访问器方法。例如将getHireDate()放回的类型是Date时：
```
public class Employee {

    private Date hireDate;

    public Date getHireDate() {
        return hireDate;
    }

}
```
因为Date有更改器方法setTime()；则能通过getHireDate()方法达到更改修改hireDate的问题：
```
Employee harry = ...
Date d = harry.getHireDate();
d.setTime(d.getTime()-365*24*60*60*1000);
```
将harry的hireDate加上一年

如果需要返回一个可变对象的引用，应该首先对它进行克隆（clone）。对象clone是指存放在另一个位置上的对象副本。
> return (Date)hireDate.clone();

### 4.3.7 基于类的访问权限
Employee类的方法可以访问Employee类的任何一个对象的私有域。
```
public boolean equals(Employeee other){
	return name.equals(other.name);
}
```
if(harry.equals(boss))既访问了harry的私有域，也访问了boss的私有域

### 4.3.8 私有方法
为了数据安全，将所有的数据域设置为私有域，大部分的方法设置为公有方法。
私有方法：当该方法只在该类使用时，比如一个方法的几个步骤。好处：不用了可以删除，而不用担心其他类依赖。

### 4.3.9 final实例域

可以将实例域定义为final。构建对象时必须初始化这样的域。

final修饰符大都应用于基本（primitive）类型域，或不可变（immutable）类的域（如果类中的每个方法都不会改变其对象，这种类就是不可变的类。例如，String类就是一个不可变的类）。





## 4.4 静态域与静态方法

### 4.4.1 静态域
将域用static修饰，表示每个类中只有一个这样的域，称为静态域。所有对象公用同一个域，即使没有对象该域也存在，因为它属于类而不是任何独立的对象，所以也叫类域。
与之相对的实例域，每一个对象都有自己的一份实例域。
```
public class Employee {
    private static int nextId;
    private  int id;
    public void setId(){
        id = nextId;
        nextId++;
    }
    
```

### 4.4.2 静态常量
静态变量用的少，静态常量用的比较多。例如Math.PI定义了π值
> public static final double PI = 3.141592653589793D;

System.out定义了
> public static final PrintStream out;

前面曾经提到过，由于每个类对象都可以对公有域进行修改，所以，最好不要将域设计为public。然而，公有常量（即final域）却没问题。因为out被声明为final，所以，不允许再将其他打印流赋给它：Cannot assign a value to final variable 'out'

### 4.4.3  静态方法
当一个方法被static修饰，表示该方法的操作不涉及对象的状态。如Math.pow(x,a);计算x的a次幂。没有隐式参数，即没有this参数。
一个静态方法不可以访问实例域，因为不能操作对象，但是可以访问自身的静态域。如下
```
    private static int nextId;
    private  int id;

    public void setId(){
        id = nextId;
        nextId++;
    }
```
调用：一般通过类名.方法名调用

在下面两种情况下使用静态方法：
* 一个方法不需要访问对象状态，其所需参数都是通过显式参数提供（例如：Math.pow）。
* 一个方法只需要访问类的静态域（例如：Employee.getNextId）。
### 4.4.4  工厂方法
用于工厂方法  todo LocalDate和NumberFormat

### 4.4.5  main方法
main方法不对任何对象进行操作。事实上，在启动程序时还没有任何一个对象。静态的main方法将执行并创建程序所需要的对象。


## 4.5 方法参数todo



## 4.6 对象构造

### 4.6.1 重载
如果多个方法（比如，StringBuilder构造器方法）有相同的名字、不同的参数，便产生了重载（overloading）。

要完整地描述一个方法，需要指出方法名以及参数类型。这叫做方法的签名（signature）。

### 4.6.1 默认域初始化

如果在构造器中没有显式地给域赋予初值，那么就会被自动地赋为默认值：数值为0、布尔值为false、对象引用为null。然而，只有缺少程序设计经验的人才会这样做。

这是域与局部变量的主要不同点。必须明确地初始化方法中的局部变量。但是，如果没有初始化类中的域，将会被自动初始化为默认值（0、false或null）。


### 4.6.3 无参数的构造器
如果在编写一个类时没有编写构造器，那么系统就会提供一个无参数构造器。这个构造器将所有的实例域设置为默认值。

如果类中提供了至少一个构造器，但是没有提供无参数的构造器，则在构造对象时如果没有提供参数就会被视为不合法。

### 4.6.4 显式域初始化
可以在类定义中，直接将一个值赋给任何域。例如：
```
public class Employee
{
    private String name = "";    
}
```
在执行构造器之前，先执行赋值操作。当一个类的所有构造器都希望把相同的值赋予某个特定的实例域时，这种方式特别有用。

初始值不一定是常量值。在下面的例子中，可以调用方法对域进行初始化。
```
    private  int id = assignId();

    public void setId(){
        id = nextId;
        nextId++;
    }
```

### 4.6.6 调用另一个构造器
关键字this引用方法的隐式参数。然而，这个关键字还有另外一个含义。如果构造器的第一个语句形如this(...)，这个构造器将调用同一个类的另一个构造器。

### 4.6.7 初始化块

前面已经讲过两种初始化数据域的方法：
*  在构造器中设置值
* 在声明中赋值
实际上，Java还有第三种机制，称为初始化块（initialization block）。在一个类的声明中，可以包含多个代码块。只要构造类的对象，这些块就会被执行。
```
    private int id;
    {
        id = nextId;
        nextId++;
    }
```
首先运行初始化块，然后才运行构造器的主体部分。

可以通过提供一个初始化值，或者使用一个静态的初始化块来对静态域进行初始化。
> private static int nextId = 1000;
当初始化的逻辑比较复杂时，使用静态代码块
```
    private static int nextId ;
    static {
        Random random = new Random();
        nextId = random.nextInt(1000);
    }
```

### 4.6.8 对象析构与finalize方法
TODO 可以为任何一个类添加finalize方法。finalize方法将在垃圾回收器清除对象之前调用。

## 4.7-4.10省略

# 第5章 继承
