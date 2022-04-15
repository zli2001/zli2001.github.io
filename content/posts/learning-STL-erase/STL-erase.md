---
weight: 1
title: "STL erase()用法"
date: 2022-03-24T21:13:25+08:00
draft: false

lastmod: 2022-03-24T21:13:25+08:00
draft: false
author: "kliiu"
authorLink: "https://kliiu.github.io"
description: "Notes of STL"
tags: ["STL"]
categories: ["STL"]

lightgallery: true
---
C++ STL中erase有三种用法：
<!--more-->
**注意可以删除一段元素，但不包括最后一个元素**
```c++
erase(element e);//删除指定元素
erase(iter it);//删除指定位置的元素，并且会返回下一个元素的地址
erase(iter begin(),iter end());//删除一段元素 前闭后开 
//会返回最后一个删除元素后一个元素的迭代器，即此处iter end()
```


