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

继承（inheritance）。利用继承，人们可以基于已存在的类构造一个新类。

反射（reflection）的概念。反射是指在程序运行期间发现更多的类及其属性的能力。

## 5.1 类、超类和子类

### 5.1.1 定义子类
关键字extends表示继承。


```
public class Manager extends Employee {
    private double bonus;

    public Manager(String name,double salary ,int year,int month,int day) {
        // 必须放在第一行
        super(name,salary,year,month,day);
        bonus = 0;
    }

    public double getBouns() {
        return bonus;
    }

    public void setBouns(double bonus) {
        this.bonus = bonus;
    }

}
```

已存在的类称为超类（superclass）、基类（base class）或父类（parent class）；新类称为子类（subclass）、派生类（derived class）或孩子类（child class）。

### 5.1.2 覆盖方法
当父类中的方法对子类不适用，比如Manager的薪水应该是salary+bonus，因此manager需要覆盖父类中的getSalary();但是不能直接获取父类中的salary私有域，这时候可以通过==super关键词==调用父类中的共有方法获取父类中的私有域，如下：
```
    @Override
    public double getSalary() {
        //return salary+bonus;not work
        //return getSalary()+bonus; 无线循环
        //通过super关键词指定父类的公有方法获取父类的私有域
        return super.getSalary()+bonus;
    }
```
### 5.1.3 子类构造器

由于Manager类的构造器不能访问Employee类的私有域，所以必须利用Employee类的构造器对这部分私有域进行初始化，我们可以通过super实现对超类构造器的调用。使用super调用构造器的语句必须是子类构造器的第一条语句。
如果子类的构造器没有显式地调用超类的构造器，则将自动地调用超类默认（没有参数）的构造器。如果超类没有不带参数的构造器，并且在子类的构造器中又没有显式地调用超类的其他构造器，则Java编译器将报告错误。

**关键字this**有两个用途：一是引用隐式参数，二是调用该类其他的构造器。
同样，super关键字也有两个用途：一是调用超类的方法，二是调用超类的构造器。

```
public class ManagerTest {
    public static void main(String[] args) {
        Employee[] employees = new Employee[3];

        Manager boss = new Manager("jack",10000,1990,1,1);
        boss.setBonus(5000);
        Employee employee1 = new Employee("employee1",8000,1990,1,1);
        Employee employee2 = new Employee("employee2",8000,1990,1,1);
        employees[0]=boss;
        employees[1]=employee1;
        employees[2]=employee2;
        for (Employee employee : employees) {
            System.out.println(employee.getSalary());
        }
    }
}
```
一个对象变量（例如，变量e）可以指示多种实际类型的现象被称为多态（polymorphism）。在运行时能够自动地选择调用哪个方法的现象称为动态绑定（dynamic binding）。

### 5.1.4 继承层次
继承并不仅限于一个层次。例如，可以由Manager类派生Executive类。由一个公共超类派生出来的所有类的集合被称为继承层次（inheritance hierarchy）。
在继承层次中，从某个特定的类到其祖先的路径被称为该类的继承链（inheritance chain）。

### 5.1.5 多态

对象变量是多态的。一个Employee变量既可以引用一个Employee类对象，也可以引用一个Employee类的任何一个子类的对象


### 5.1.6 理解方法调用
TODO看书
重载解析（overloading resolution）  
静态绑定（static binding） 动态绑定  
方法表（method table）  

### 5.1.7 阻止继承：final类和方法

不允许扩展的类被称为final类。如果在定义类的时候使用了final修饰符就表明这个类是final类
类中的特定方法也可以被声明为final。如果这样做，子类就不能覆盖这个方法（final类中的所有方法自动地成为final方法）。

域也可以被声明为final。对于final域来说，构造对象之后就不允许改变它们的值了。不过，如果将一个类声明为final，只有其中的方法自动地成为final，而不包括域。


### 5.1.8 强制类型转换
将一个子类的引用赋给一个超类变量，编译器是允许的。但将一个超类的引用赋给一个子类变量，必须进行类型转换

instanceof操作符：if(staff[1] instanceof Manager){boss = (Manager)staff[1]}

### 5.1.9 抽象类

使用abstract关键字,定义一个方法，表示子类来实现，这样就完全不需要实现这个方法了。

为了提高程序的清晰度，包含一个或多个抽象方法的类本身必须被声明为抽象的。

除了抽象方法之外，抽象类还可以包含具体数据和具体方法。

类即使不含抽象方法，也可以将类声明为抽象类。抽象类不能被实例化。也就是说，如果将一个类声明为abstract，就不能创建这个类的对象。

可以定义一个抽象类的对象变量，但是它只能引用非抽象子类的对象。

### 5.1.10 受保护访问
人们希望超类中的某些方法允许被子类访问，或允许子类的方法访问超类的某个域。为此，需要将这些方法或域声明为protected。
下面归纳一下Java用于控制可见性的4个访问修饰符：
1. 仅对本类可见——private。
2. 对所有类可见——public。
3. 对本包和所有子类可见——protected。
4. 对本包可见——默认（很遗憾），不需要修饰符。

## 5.2 Object：所有类的超类

只有基本类型（primitive types）不是对象，例如，数值、字符和布尔类型的值都不是对象。

### 5.2.1 equals方法

```
    public boolean equals(Object var1) {
        return this == var1;
    }
```

Object类中的equals方法用于检测一个对象是否等于另外一个对象。在Object类中，这个方法将判断两个对象是否具有相同的引用。如果两个对象具有相同的引用，它们一定是相等的。

然而在很多类中，这么比较是没有意义的，比如在Employee类中，如果两个雇员的名字、薪水和雇佣日期一样，就认为他们是相等的（实际应该比较id），所以在Employee类中要覆写equals方法：
```
    @Override
    public boolean equals(Object otherObject) {

        if (this == otherObject) {
            return true;
        }
        if (otherObject == null) {
            return false;
        }
        if (getClass() != otherObject.getClass()) {
            return false;
        }
        Employee other = (Employee) otherObject;

        return Objects.equals(this.name, other.name) && Objects.equals(this.hireDate, other.hireDate) && salary == other.salary;
    }
```
在子类中定义equals方法时，首先调用超类的equals。如果检测失败，对象就不可能相等。如果超类中的域都相等，就需要比较子类中的实例域。
```
public class Manager extends Employee {
    @Override
    //使用@Override的好处：可以避免类型错误 ，如将Object 改为Employee时会报错
    public boolean equals(Object otherObject) {
        if (!super.equals(otherObject)) {
            return false;
        }
        Manager other = (Manager) otherObject;
        return bonus == other.bonus;
    }
}
```

### 5.2.2 相等测试与继承
对于数组类型的域，可以使用静态的Arrays.equals方法检测相应的数组元素是否相等。

### 5.2.3 hashCode方法

散列码（hash code）是由对象导出的一个整型值。散列码是没有规律的。
如果x和y是两个不同的对象，x.hashCode( )与y.hashCode( )**基本上**不会相同。
如果x和y是相同的对象，那它们的hashcode一定相同。

```
    @Override
    public int hashCode(){
        return Objects.hash(name,hireDate,salary);

    }
```
如果存在数组类型的域，那么可以使用静态的Arrays.hashCode方法计算一个散列码，这个散列码由数组元素的散列码组成。
实际上，Objects.hash(Object...obj)也是调用的Arrays.hashCode




为什么重写equals()方法后就一定得重写hashcode()方法：
hashCode()方法的定义就是：equals方法认定的两个相同的对象，一定得有相同的hashCode

https://zhuanlan.zhihu.com/p/50206657


## 5.3 泛型数组列表ArrayList

一旦确定了数组的大小，改变它就不太容易了。在Java中，解决这个问题最简单的方法是使用Java中另外一个被称为ArrayList的类。它使用起来有点像数组，但在添加或删除元素时，具有自动调节数组容量的功能，而不需要为此编写任何代码。

ArrayList是一个采用类型参数（type parameter）的泛型类（generic class）。
为了指定数组列表保存的元素对象类型，需要用一对尖括号将类名括起来加在后面，例如，ArrayList<Employee> staff = new ArrayList<>(100)。

数组列表管理着对象引用的一个内部数组。最终，数组的全部空间有可能被用尽。如果调用add且内部数组已经满了，数组列表就将自动地创建一个更大的数组，并将所有的对象从较小的数组中拷贝到较大的数组中。

如果已经清楚或能够估计出数组可能存储的元素数量，就可以在填充数组之前调用
> ensureCapacity:staff.ensureCapacity(100);

一旦能够确认数组列表的大小不再发生变化，就可以调用trimToSize方法。这个方法将存储区域的大小调整为当前元素数量所需要的存储空间数目。垃圾回收器将回收多余的存储空间。

## 5.4 对象包装器与自动装箱

有时，需要将int这样的基本类型转换为对象。所有的基本类型都有一个与之对应的类。例如，Integer类对应基本类型int。通常，这些类称为包装器（wrapper）。这些对象包装器类拥有很明显的名字：Integer、Long、Float、Double、Short、Byte、Character、Void和Boolean（前6个类派生于公共的超类Number）。对象包装器类是不可变的，即一旦构造了包装器，就不允许更改包装在其中的值。同时，对象包装器类还是final，因此不能定义它们的子类。



假设想定义一个整型数组列表。而尖括号中的类型参数不允许是基本类型，也就是说，不允许写成ArrayList<int>。这里就用到了Integer对象包装器类。我们可以声明一个Integer对象的数组列表。

> ArrayList<Integer> list = new ArrayList<>();

调用list.add(3)自动变成list.add(Integer.valueOf(3));这种变换被称为自动装箱（autoboxing）。

当将一个Integer对象赋给一个int值时，将会自动地拆箱: int n = list.get(i);int n = list.get(i).intValue;



注意：TODO 比较基本类型时使用==，比较包装器类型时，使用equals方法：

自动装箱规范要求boolean、byte、char≤127，介于-128～127之间的short和int被包装到固定的对象中。

如果在一个条件表达式中混合使用Integer和Double类型，Integer值就会拆箱，提升为double，再装箱为Double



装箱和拆箱是编译器认可的，而不是虚拟机。编译器在生成类的字节码时，插入必要的方法调用。虚拟机只是执行这些字节码

TODO 由于Java方法都是值传递



## 5.5 参数数量可变的方法

如printf方法：

```
        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < 10; j++) {
                System.out.printf("%6d--%6s",i*20+j+1,"sss"+i);
            }
            System.out.println();
        }
```

格式字符串%6d :右对齐6位整数

## 5.6 枚举类todo
## 5.7反射 todo






