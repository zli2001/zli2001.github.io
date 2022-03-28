---
title: "Java构造函数默认参数"
date: 2022-03-24T17:13:25+08:00
draft: false
tags: ["java","code"]
series: ["JavaLearning"]
categories: ["Coding"]
---
java中的构造函数无指定默认参数功能，但可以通过方法重载实现：
<!--more-->
```java
class  Lader{
    private double up, down, height, size;
        double laderSize(){
        //返回面积
        size=(up+down)*height/2;
        return size;
    }
//Java支持方法重载，间接实现构造函数默认参数，
// 因此没有如C++中默认参数的功能
    Lader(double u, double d){
        up=u;
        down=d;
    }

    Lader(double u, double d, double h){
        up=u;
        down=d;
        height=h;
    }
}

public class LaderCircle{
    Lader a = new Lader(1.0,2.0);
}
```