---
title: java设计模式
date: 2022-03-10 10:47:50
tags: 设计模式
description:
categories: 设计模式
---


### OO原则
封装变化的部分和不变的部分独立出来  
多用组合，少用继承  
针对接口编程，不针对实现编程


### 一、单例模式

> 场景：当一个程序中的一个类只能有一个实例对象时，如线程池

> 定义：单例模式确保一个类只能有一个实例，并且提供一个全局访问点返回该对象。

> 思路：  
单例的类，不能被创建对象，所以将构造方法private  
类中有个public的静态方法返回该对象    
用一个静态变量指向该对象

todo:和静态类的作用区别

####  懒汉式
> 延迟加载，用的时候才创建，提高效率，会有线程安全问题

```java
public class LanHanDemo {
    //1.私有的构造方法,使得其他类无法创建其对象，只能内部创建
    private LanHanDemo(){
    }
    //2.私有的对象
    private static LanHanDemo lanHanDemo;

    //3.公开的静态的方法返回对象
    public static  LanHanDemo getInstance(){
        //延迟加载,线程不安全
        if (lanHanDemo==null){
            lanHanDemo = new LanHanDemo();
        }
        return lanHanDemo;
    }

}
```


#### 饿汉式

> JVM在加载这个类时立马创建唯一的单例实例，不会有线程安全问题


```java
public class EHanDemo {
    private EHanDemo(){
        
    }

    private static EHanDemo eHanDemo = new EHanDemo();

    public static EHanDemo getInstance(){
        return eHanDemo;
    }
}
```


#### 懒汉式(线程安全)

> 使用synchronized关键词解决线程安全问题,效率低，每一次都需要判断锁


```java
public class LanHanDemo {
    //1.私有的构造方法,使得其他类无法创建其对象，只能内部创建
    private LanHanDemo(){
      
    }
    //2.私有的对象
    private static LanHanDemo lanHanDemo;

    //3.公开的静态的方法返回对象
    //通过加锁，使得线程安全，但是效率会有问题
    public static synchronized LanHanDemo getInstance(){
        //延迟加载,线程不安全
        if (lanHanDemo==null){
            lanHanDemo = new LanHanDemo();
        }
        return lanHanDemo;
    }
}
```


#### 懒汉式(使用双重校验锁)

> （线程安全，第一次创建时才需要加锁，效率高）

```java
/**
 * 懒汉式加上双重校验锁解决多线程不安全问题
 */
public class DoubleCheckedLockingDemo {
    private DoubleCheckedLockingDemo(){
    }

    /**
     * 为啥加 volatile，防止指令充排？
     */
    private static volatile DoubleCheckedLockingDemo instance;

    public static DoubleCheckedLockingDemo getInstance(){
    //此处还是会有线程不安全问题，所以在锁里再一次判断
        if (instance==null){
            synchronized (DoubleCheckedLockingDemo.class){
                if (instance==null){
                    instance = new DoubleCheckedLockingDemo();
                }
            }
        }
        return instance;
    }


}
```
#### 使用静态内部类的方式实现

> 静态内部类不会在单例加载时就加载，而是在调用getInstance()方法时才进行加载，达到了类似懒汉模式的效果，而这种方法又是线程安全的。


```java
/**
 * 静态内部类实现单例：线程安全
 */
public class InnerClassSingletonDemo {

    private InnerClassSingletonDemo(){
        System.out.println("静态内部类构造");
    }
    public static InnerClassSingletonDemo getInstance(){
        return SingletonHolder.instance;
    }
    private static class SingletonHolder{
        private static InnerClassSingletonDemo instance = new InnerClassSingletonDemo();
    }
}
```
#### 使用枚举的方式

> 这种方式是Effective Java作者Josh Bloch 提倡的方式，它不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象

```java
/**
 * 使用enum的方式构造单例
 */
public enum SingletonEnumDemo {
    SINGLETON_ENUM_DEMO();
    public void printInfo(){
        System.out.println("使用枚举构造单例");
    }
}
```



#### 其他 ThreadLocal和Cas的方式


### 二、策略模式(Strategy)
情景：比如去旅行，可以选择开车，火车，飞机不同得出行方式，这些就是不同的策略，也叫算法，这些策略可以相互替代。

定义：策略模式定义一族算法，分别封装起来，并且可以相互替代，让算法的实现独立于使用算法的客户。

实现：
Strategy接口：所有的算法都要实现此接口  
ConcreteStrategy类：实现Strategy接口，具体的算法实现  
Context：上下文对象，决定选择何种算法

优点：

我们之前在选择出行方式的时候，往往会使用if-else语句，也就是用户不选择A那么就选择B这样的一种情况。这种情况耦合性太高了，而且代码臃肿，有了策略模式我们就可以避免这种现象，
策略模式遵循开闭原则，实现代码的解耦合。扩展新的方法时也比较方便，只需要继承策略接口就好了
上面列出的这两点算是策略模式的优点了，但是不是说他就是完美的，有很多缺点仍然需要我们去掌握和理解，

缺点：
客户端必须知道所有的策略类，并自行决定使用哪一个策略类。
策略模式会出现很多的策略类。
context在使用这些策略类的时候，这些策略类由于继承了策略接口，所以有些数据可能用不到，但是依然初始化了


```java
/**
 * 抽象策略类：出行策略的抽象接口
 */
public interface TravelStrategy {
    /**
     * 出行方式，每个算法都要实现自己的出行方式
     */
    void travelMethod();
}
```

```java
/**
 * 具体策略类：ConcreteStrategy
 */
public class TravelByTrain implements TravelStrategy {
    @Override
    public void travelMethod() {
        System.out.println("坐火车去大理。。。");
    }
}
```

```java
public class TravelByPlane implements TravelStrategy {
    @Override
    public void travelMethod() {
        System.out.println("坐飞机去。。。");
    }
}

```

```java
/**
 * 游客：用来操作策略的上下文环境
 */
public class Traveler {
    TravelStrategy travelStrategy;

    void setTravelStrategy(TravelStrategy travelStrategy){
        this.travelStrategy = travelStrategy;
    }
    public void travel(){
        travelStrategy.travelMethod();
    }
    public Traveler(TravelStrategy travelStrategy){
        this.travelStrategy = travelStrategy;
    }

    public static void main(String[] args) {
        Traveler traveler = new Traveler(new TravelByPlane());
        traveler.travel();
        traveler.setTravelStrategy(new TravelByTrain());
        traveler.travel();
    }
}
```




### 三、观察者模式

情景：有一个气象站（WeatherData），拥有一些数据，比如温度，湿度等，有多个布告板，需要从气象站获得数据，气象站数据更改了，布告板的数据也要更新。
相当于订阅发布

定义：观察者模式定义了对象之间的一对多关系，当一个有状态的对象(被观察者)改变时，多个依赖的对象（观察者）也能收到通知并自动更新。

自定义实现：
1. 定义一个被观察者（主题）抽象接口 Subject，里面有添加观察者、移除观察者、更新所有观察者的方法

```java
/**
 * 被观察者抽象接口
 */
public interface Subject {
    //2.注册一个观察者
    void registerObserver(Observer observer);
    //3.删除一个观察者
    void removeObserver(Observer observer);
    //4.更新所有观察者
    void notifyAllObserver();
}
```
2. 定义一个具体的被观察者实现


```java
public class WeatherData implements Subject {

    private float temperature;
    private float humidity;
    private float pressure;

    /**
     * 模拟数据更改
     */
    public void setWeatherData(float temperature,float humidity,float pressure){
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        notifyAllObserver();
    }

    private ArrayList observerList ;

    public WeatherData(){
        observerList = new ArrayList();
    }
    @Override
    public void registerObserver(Observer observer) {
        observerList.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observerList.remove(observer);
    }

    @Override
    public void notifyAllObserver() {
        for (Object o : observerList) {
            Observer observer = (Observer)o;
            observer.update(temperature,humidity,pressure);
        }

    }
}
```

3. 定义一个观察者接口Observer，里面有个update()，所有的观察者实现此接口
   ，被观察者通过此接口更新所以观察者。


```java
/**
 * 观察者抽象接口，所以具体观察者需要实现此接口
 */
public interface Observer {
    void update(float temperature,float humidity,float pressure);
}
```
4.定义一个具体的观察者类，实现update方法，注册被观察者（主题）接口


```java
public class CurrentConditions implements Observer ,Display{
    private float temperature;
    private float humidity;
    private float pressure;

    @Override
    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        this.display();

    }
    public CurrentConditions(Subject subject){
        subject.registerObserver(this);
    }

    @Override
    public void display() {
        System.out.println(temperature+"===="+humidity+"===="+pressure);
    }
}
```
通过java提供的Observable类和Observer接口实现  
类中自带了addObserver、deleteObserver和notifyObservers接口，维护了一个observers的vector



```java
import java.util.Observable;

public class WeatherDataByJava extends Observable {
    private float temperature;
    private float humidity;
    private float pressure;

    public float getTemperature() {
        return temperature;
    }

    public float getHumidity() {
        return humidity;
    }

    public float getPressure() {
        return pressure;
    }

    /**
     * 模拟数据更改
     */
    public void setWeatherData(float temperature,float humidity,float pressure){
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        //数据更改前将setChanged设置为true
        setChanged();
        notifyObservers("hahah");
    }

    public WeatherDataByJava(){
    }
}
```


```java
import java.util.Observable;

public class CurrentConditionsByJava implements java.util.Observer ,Display{
    private float temperature;
    private float humidity;
    private float pressure;

    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        this.display();

    }
    public CurrentConditionsByJava(Observable o){
        o.addObserver(this);
    }

    @Override
    public void display() {
        System.out.println(temperature+"===="+humidity+"===="+pressure);
    }

    @Override
    public void update(Observable o, Object arg) {
        if (o instanceof WeatherDataByJava){
            WeatherDataByJava weatherData = (WeatherDataByJava)o;
            this.temperature = weatherData.getTemperature();
            this.humidity = weatherData.getHumidity();
            this.pressure = weatherData.getPressure();
            System.out.println(arg);
            display();
        }
    }
}
```
问题：不能按顺序通知，java自带的是一个类，不利于扩展


### 四、装饰者模式（结构性）

>定义：在不改变一个类的类型的情况下，动态的将责任或者功能加到该类上，提供了比继承更有弹性的替代方案

>场景，一种饮料可以添加多种口味的


```java
/**
 * 抽象组件：饮料接口
 */
public abstract class Beverage {
    String description = "";

    public String getDescription(){
        return description;
    }


    public abstract double cost();
}

```

```java
/**
 * 具体组件：浓缩咖啡
 */
public class Espresso extends Beverage {

    public Espresso(){
        description = "Espresso浓缩咖啡";
    }

    @Override
    public double cost() {
        return 8;
    }
}
```

```java
/**
 * 抽象装饰类：调料接口
 */
public class CondimentDecorator extends Beverage {
    @Override
    public String getDescription() {
        return null;
    }

    @Override
    public double cost() {
        return 0;
    }
}
```


```java
/**
 * 具体装饰者：加了摩卡的饮料
 */
public class MochaDecorator extends CondimentDecorator {
    private Beverage beverage;

    public MochaDecorator(Beverage beverage){
        this.beverage=beverage;
    }
    public double cost(){
        return 2+beverage.cost();
    }
    public String getDescription(){
        return beverage.getDescription()+"加了mocha";
    }
}
```

```java
/**
 * 咖啡馆,测试类
 */
public class StarbuzzCoffee {
    public static void main(String[] args) {
        Beverage beverage = new Espresso();
        beverage  = new MochaDecorator(beverage);
        beverage  = new MochaDecorator(beverage);
        System.out.println(beverage.getDescription());
        System.out.println(beverage.cost());
    }
}
```




### 五、工厂模式

> 简单工厂模式（Simple factory）:并不是一个设计模式，把创建具体对象的代码单独的放到一个类中，客户端不用关心创建的具体实现，这个类就是简单工厂，被创建的对象有共同的父类。

情景:通过ifelse或者swith判断选择创建dell或者hp的键盘


```java
public class ClientDemo {

    public KeyBoard getKeyBoard(String type) {
        switch (type) {
            case "dell":
                return new DellKeyboard();
            case "hp":
                return new HpKeyboard();
        }
        return null;
    }

    public static void main(String[] args) {
        ClientDemo clientDemo = new ClientDemo();
        clientDemo.getKeyBoard("dell").display();
    }
}


public class DellKeyboard implements Keyboard{
    @Override
    public void display() {
        System.out.println("Dell 的键盘");
    }
}

public class HpKeyboard implements Keyboard{
    @Override
    public void display() {
        System.out.println("Hp 的键盘");
    }
}

public interface Keyboard {
    void display();
}
```
实现：  
新建一个简单工厂，通过工厂获取对象
```java
public class SimpleKeyboardFactory {

    public Keyboard getKeyBoard(String type){
        switch (type) {
            case "dell":
                return new DellKeyboard();
            case "hp":
                return new HpKeyboard();
        }
        return null;
    }
}
```

```java
public class ClientDemo {

    private SimpleKeyboardFactory keyboardFactory;

    public ClientDemo(SimpleKeyboardFactory keyboardFactory){
        this.keyboardFactory = keyboardFactory;
    }

    public Keyboard getKeyBoard(String type) {
    
    //通过简单工厂来获取对象
        return keyboardFactory.getKeyBoard(type);
    }

    public static void main(String[] args) {
        ClientDemo clientDemo = new ClientDemo(new SimpleKeyboardFactory());
        clientDemo.getKeyBoard("dell").display();
    }
}
```
问题： 如果新加入一个产商的键盘，需要更改工厂类，不符合开闭原则

升级：将简单键盘工厂抽象成接口，由具体的工厂子类决定生成何种键盘

> 工厂方法模式定义一个创建对象的接口，但是由子类来决定实例化的对象是哪一个，即把类的实例化推迟到子类


定义一个抽象的工厂方法，让子类实现该方法制造产品
```java
/**
 * 设计模式：工厂方法
 */
public interface KeyboardFactory {

    Keyboard getKeyboard();
}
```

```java
public class HpKeyboardFactory implements KeyboardFactory {
    @Override
    public Keyboard getKeyboard() {
        return new HpKeyboard();
    }
}
```

```java
public class DellKeyboardFactory implements KeyboardFactory {
    @Override
    public Keyboard getKeyboard() {
        return new DellKeyboard();
    }
}
```

```java
public class ClientDemo {

    private KeyboardFactory keyboardFactory;

    public ClientDemo(KeyboardFactory keyboardFactory){
        this.keyboardFactory = keyboardFactory;
    }

    public Keyboard getKeyBoard(String type) {

        return keyboardFactory.getKeyboard();
    }

    public static void main(String[] args) {
        //通过指定具体的工厂子类来决定生产何种键盘
        ClientDemo clientDemo = new ClientDemo(new DellKeyboardFactory());
        clientDemo.getKeyBoard("dell").display();
    }
}
```

> 抽象工厂模式：提供一个接口，用于创建相关或者依赖对象的家族，而不需要明确具体类。

### 六、代理模式

> 情景：火车票代售处就是火车站的代理

> 定义：为其他对象提供代理，以控制对这个对象的访问  
代理可以在被代理的对象上加一些功能或者减少一些功能

常见的代理模式：虚拟代理、远程代理、保护代理、只能代理

两种代理的实现方式： 静态代理和动态代理

>静态代理：代理和被代理的对象在代理之前是确定的，他们都实现相同的接口或者继承相同的抽象类

实现方式：
1. 代理类继承被代理类，扩展功能

```java
public interface Movable {
    void run();
}
```



```java
public class Car implements Movable {
    @Override
    public void run() {
        System.out.println("汽车开始行使。。。。");
        try {
            Thread.sleep(new Random().nextInt(1000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
/**
 * car 的代理类:继承实现
 */
public class CarTimerProxy extends Car{
    @Override
    public void run(){
        System.out.println("开始计时。。");
        long startTime = System.currentTimeMillis();
        super.run();
        long endTime = System.currentTimeMillis();
        System.out.println("计时结束。。用时："+(endTime-startTime)+"毫秒");
    }
}
```


```java
public class ClientDemo {
    public static void main(String[] args) {
        Movable carTimerProxy = new CarTimerProxy();
        carTimerProxy.run();
        System.out.println("==============");
        Movable car = new Car();
        Movable movable2 = new CarLogProxy(carTimerProxy);
        movable2.run();
    }
}
```


2. 代理类和被代理类实现同一接口，扩展功能，**聚合实现更方便扩展**

```java
/**
 * car的代理类：聚合（接口）实现
 */
public class CarLogProxy implements Movable {

    private Movable movable;

    public CarLogProxy(Movable movable){
        this.movable = movable;
    }
    @Override
    public void run() {
        System.out.println("开始记录log。。");
        movable.run();
        System.out.println("开始记录log。。");
    }
}
```

> 问题:上述的CarTimerProxy只是实现了对Car的时间代理，如果现在有个train也要时间代理，需要重新写个TrainTimerProxy？

> 动态代理：根据不同的类的不同的方法，动态的建立代理类

> 实现方式：1.通过jdk的java.util.reflect.InvocationHandler实现的动态代理
在代理类和被代理类之间加了Proxy 和InvocationHandler接口

1.新建时间代理TimerHandler implements InvocationHandler，实现invoke方法，在invoke里实现具体的代理逻辑

```java
/**
 * 定义一个handler实现 java.lang.reflect.InvocationHandler
 * 实现invoke方法
 */
public class TimerHandler implements InvocationHandler {
    /**
     * 被代理对象
     */
    private Object object;

    public TimerHandler(Object object) {
        this.object = object;
    }

    /**
     * @param proxy 被代理对象?
     * @param method 被代理对象的方法
     * @param args 方法的参数
     * @return 方法的返回值
     * @throws Throwable
     */
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("开始计时。。");
        long startTime = System.currentTimeMillis();
        method.invoke(object,args);
        long endTime = System.currentTimeMillis();
        System.out.println("计时结束。。用时："+(endTime-startTime)+"毫秒");
        return null;
    }
}
```
2.新建被代理类（Car)和其接口（Movable）

3.通过Proxy.newProxyInstance获取动态代理，强转成Movable，调用run方法

```java
public class ClientDemo {
    public static void main(String[] args) {
        //1. 新建一个被代理对象
        Movable car = new Car();
        //2.动态获取代理
        TimerHandler timerHandler = new TimerHandler(car);
        Class<? extends Movable> carClass = car.getClass();
        Movable o = (Movable)Proxy.newProxyInstance(carClass.getClassLoader(), carClass.getInterfaces(), timerHandler);
        o.run();
    }
}
```

> JDK动态代理只能代理实现某些接口的类，cglib动态代理，是针对某些类来实现动态代理的，通过实现该类的子类，拦截父类中方法，因此不能代理final修饰的类

todo 不是很懂
```java
public class LogCglibProxy implements MethodInterceptor {

    private Enhancer enhancer = new Enhancer();

    public Object getProxy(Class clazz){
        enhancer.setSuperclass(clazz);
        enhancer.setCallback(this);
        return enhancer.create();
    }
    /**
     * @param object 目标类的实例
     * @param method 目标方法的反射对象
     * @param args 方法的参数
     * @param methodProxy 代理类
     * @return
     * @throws Throwable
     */
    @Override
    public Object intercept(Object object, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        //代理类调用父类的方法
        System.out.println("cglib开始记录log。。");
        methodProxy.invokeSuper(object,args);
        System.out.println("cglib结束记录log。。");
        return null;
    }
}
```

```java
public class Train {

    public void run() {
        System.out.println("火车开始行使。。。。");
        try {
            Thread.sleep(new Random().nextInt(1000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```


```java
        //通过cglib实现动态代理
        LogCglibProxy logCglibProxy = new LogCglibProxy();
        Train proxy = (Train)logCglibProxy.getProxy(Train.class);
        proxy.run();
```
