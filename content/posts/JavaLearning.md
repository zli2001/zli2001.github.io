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
用static 修饰的称作类变量、类方法（static 方法、静态方法）。可以通过类名调用

类方法不能操作实例变量。在类创建对象之前实例成员还没有分配内存。
也不能调用类中的实例方法

其他则成为实例变量、实例方法。

实例方法可以操作实例变量、类变量，调用实例方法、类方法（不包括构造方法）

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

## 继承

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
- 所有的方法都为abstract
- 100% pure abstract class
- 一个class可以implement 多个interface

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

