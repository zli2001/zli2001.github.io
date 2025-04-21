---

title: "Python Learning Notes"
date: 2020-05-28T10:09:58+08:00
draft: false
lastmod: 2021-05-28T10:09:58+08:00

author: "zli2001"
authorLink: "https://zli2001.github.io"
hiddenFromSearch: false
tags: ["code","Python"]
categories: ["Coding"]


lightgallery: true
---
Python learning notes
<!--more-->

@[TOC](基础)
# ·数据类型和变量
 - 坚持使用四个空格的缩进！
 - '''...'''的用法：可以表示**多行内容**（交互输入)
 -   可以直接用True False表示**布尔值** 进行运算（区分大小写）
 - 一个变量可以多次赋值，且是**不同类型**的变量——*动态语言*
 - 通常用大写代表**常量** （习惯）事实上它仍然是一个变量
 - 两种除法：/ 和 //
 - 整数没有大小限制
## 

# 
## 字符串
修改大小写、合并、空白符、删除空白
str()函数避免类型错误

***"Now is better than never!!!"***
# ·列表
fruits=['apple','orange','grape']
对列表元素的

## 增加
插入到任意位置insert()以及添加到末尾append()
## 删除
del/pop()/remove() ---3种方式
    **· del fruits[0]**

## 排序
tour.sort()&sorted(fruits)---前者永久
可传递参数（reverse=Ture）则与字母顺序相反
## 反序
reverse()
## 确定长度
len(fruits)----start from 1
返回最后一个元素可用fruits[-1]索引

# ·操作列表
**for 循环游历数组** 
*for fruit in fruits:#注意冒号
	print(fruit)#必须**缩进***
	**range()生成一系列数值**
	*numbers = list(range(1,5))-----输出结果为1234*
	*range(2,11,1)----1为步长*
	**简单的统计计算**
	*min(numbers),max(),sum()*
	**列表解析**
	squares=[value**2 for value in range(1,11)]# **为平方运算
**切片**
print(fruit[0:2])
print(fruit[-3:])---最后三个
负数表示离列表末尾相应距离的元素
切片也可用于for循环
# split函数用于分割字符串
例子转自https://www.cnpython.com/qa/691478
![在这里插入图片描述](https://img-blog.csdnimg.cn/63f4ab520fea4b07bcff0bc7c961754c.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)
# numpy.where() 函数
对于满足和不满足条件的两种情况输出不同的结果
# pandas.apply() 函数
“函数作为一个对象，能作为参数传递给其它函数，也能作为函数的返回值。”
