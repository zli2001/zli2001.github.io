---
title: "Java Learning"
date: 2022-03-28T10:09:58+08:00
draft: false
tags: ["java","code"]
series: ["JavaLearning"]
categories: ["Coding"]
---
<!--more-->

## 类与对象
### 方法
```java
public class 三_1 {
int m=10,n;//正确，成员变量的操作只能放在方法中，声明时可以赋初值
n=100;//报错，赋值只能放在方法中
}
```
### 构造方法
默认有一个无参数构造函数，但是如果指定了有参数的构造函数，**就需要自己写一个无参数的**。
### this
表示某个对象，可以出现在实例方法和构造方法中
### 局部变量
- 局部变量没有默认值，必须初始化。
- 成员变量有默认值，可以不初始化。
### 实例成员与类成员
- 用static 修饰的称作类变量、类方法（static 方法、静态方法）。可以通过类名调用

- 类方法
    >不能操作实例变量。在类创建对象之前实例成员还没有分配内存。
    
    >也不能调用类中的实例方法

- 其他则成为实例变量、实例方法。

- 实例方法可以操作实例变量、类变量，调用实例方法、类方法（不包括构造方法）
- static 变量在实例创建之前就已经被初始化

### 可变参数
```java
class E_4{

    public static void main(String args[]){
        f(1,2);
        f(-2,-2,-3,-4);
    }
    public static void f(int ... x){//x是可变参数的代表，代表若干个int型参数
        for(int i=0;i<x.length;i++)
            System.out.println(x[i]);//类似数组，代表第i个参数
    }
}
```

## Java Library
### ArrayList
### format
- &d decimal:
>`format("%d",42);`
> 
>42
- %f floating point：
>`format("%.3f",42.00000000);`
>
>42.000
- %x hexadecimal十六进制：
>`format("%.3f",42.00000000);`
>
>42.000
- %c character
>`format("%c",42);
>//42表示 char "*"`
>*
>
- 多个argument
- `format("The rank is %,d out of &,.2f",one,two);`
    > The rank is 20,456,654 out of 100,567,890.25

## 子类与继承

子类is a 父类
```java
class Sum{
    //...
}
class Average extends Sum{
    //...Sum的子类
}
```

- **Single Parent**
- 子类不继承父类的构造方法（默认调用父类不带参数的构造方法），可以使用super()调用父类的其他构造方法

- super()必须是构造方法的第一个语句

- 当创建一个子类，总是先调用父类的构造方法再调用子类的

- 默认所有的类继承自Object类
- instanceof:左边的操作元素是右面的类或其子类所创建的对象时为true
### 子类的继承性
在同一包：除了private

不同包：除了private 和 default，即继承protected 和public

权限由高到低：public protected default private 
### 成员变量的隐藏与方法重写
#### 成员变量的隐藏
子类声明的成员变量名字与从父类继承来的成员变量名字相同，类型可以不同
#### 重写 override
- 子类方法必须与父类保持类型一致，
- 与重载 overload不同
- 不能降低方法的访问权限，只能提升
- 不可以把父类的实例方法重写为类（static）方法，也不可以把父类的类方法重写为实例方法。
### super
super必须是子类构造方法的第一句

调用被隐藏的成员变量与方法

### final
- final类不能被继承

- final方法不能被重写

- final变量为常量，声明时必须指定值。
- 不可以用final修饰构造方法
- final static 变量：
    ```java
    //初始化final static 变量的两种方式
    class Foo{
        final static int X=25;
        //final: value doesn't change
        //static: don't need an instance of object
    }
    //or
    class Bar{
        static final double X;
        static{
        //static initializer 在类加载前就执行的代码块
            X=1.0;
        }
    }
    ```

### 上转型对象
### 多重继承
- java不支持多重继承
- 但是可以用`interface`实现多重继承的功能

### 接口 interface
```java
public interface Pet{
    void beFriendly();
    public abstract void play();
    //接口所有方法默认为public abstract,可以省略
}
public class Dog extends Canine implements Pet{
    //...
}
```
- 允许除了抽象方法和常量以外的：
> default 实例方法（必须是public）（不可以省略default,可以省略public）
> （不可以定义default的static方法）//jdk8
> 
> static 方法//jdk8
> 
> private 方法//jdk9
> 
1. 一个class可以implements 多个interface
>`class Dog extends Animal implements Eatable,Sleepable{}`
2. 重写接口中的方法
- 重写default方法需要去掉default
- 类不拥有接口的static方法和private方法
- 除了private以外，其他方法默认为public，重写时不可省略，否则降低了访问权限
3. 使用接口中的常量和static方法
- 可以用接口名访问接口的常量、调用接口中的static方法
4. 接口的细节
- public接口可以被任意类实现（implements）
- 友好接口（不加public）只能被同包中的类实现
- 父类实现接口即子类也实现了，不必再implements
- 接口可以继承
## 多态性
- 静态多态性
>方法重载
- 动态多态性
>   将子类对象的引用放进父类：
>   上转型对象a: 不能访问子类新增的方法和变量
>   `A a = new B()//B为子类`

## 抽象
### 抽象类
如Animals类。不应该被实例化，因此定义为抽象类，让其无法被new

抽象类可以有抽象方法和非抽象方法
### 抽象方法
必须被重写的方法，which has no body!
没有大括号

`public abstract void eat();`

- 抽象方法必须放在抽象类里。
- 必须重写所有的抽象方法

- 为什么要有抽象方法？为了多态性，所有的子类都有这个方法




## 异常处理
```java
try{
    turnOvenOn();
    x.bake();
}catch (BakingException ex){
    ex.printStackTrace();//recover code
}finally{
    turnOvenOff();
}

```

## GUI
Three ways to put things on GUI:
1. put widgets on a frame
2. draw 2D graphics on a widget
3. put a JPEG on a widget