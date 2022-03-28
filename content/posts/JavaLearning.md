---
title: "Java Learning"
date: 2022-03-28T10:09:58+08:00
draft: false
tags: ["java","code"]
series: ["JavaLearning"]
categories: ["Coding"]
---
<!--more-->
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

