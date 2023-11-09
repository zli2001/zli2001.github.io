# Debug Configuration如何配置（已有项目）

【CLion】Debug Configuration如何配置（已有项目）
<!--more-->



# 项目场景：

**CLion C++ coding**

# 问题描述：

**CLion打开已有项目，运行一个简单的C++ 输出hello文件，debug是应该自动配置的，无需手动添加**，但我的项目debug configuration一直没有自动配置。

# 原因分析：

cpp文件名不能有中文！！要改为英文。
也许不会有提示或者报错，但当文件名有中文时，debug configuration就会一直处于空白。其实就是CMakeLists.txt文件的问题

# 解决方案：

新建一个project，然后把CMakeLists.txt里的内容复制过来并更改对应名称
更改好以后记得reload changes.
```cpp
cmake_minimum_required(VERSION 3.21)
project(Leetcode)

set(CMAKE_CXX_STANDARD 11)

add_executable(Leetcode stackqueue.cpp)
```
如果没有报错，出现以下消息则说明配置成功了。

![配置图片1](https://img-blog.csdnimg.cn/52769635323c4fb68c7fc1540d929c03.png)
应该可以看到debug 图标变绿并自动出现一个configuration。

![成功效果](https://img-blog.csdnimg.cn/1670e9f703d74524921d31dc4132fb1b.png)

**成功运行**
![运行效果](https://img-blog.csdnimg.cn/61cad843acbd4f8881fcbfce87413a3a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YWw5bee5omL5Y23,size_20,color_FFFFFF,t_70,g_se,x_16)


