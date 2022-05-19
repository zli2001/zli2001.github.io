---

title: "虚拟机可以上网，但宿主机不能的解决方案"
date: 2021-08-16T10:09:58+08:00
draft: false
lastmod: 2021-06-30T10:09:58+08:00

author: "kliiu"
authorLink: "https://kliiu.github.io"
hiddenFromSearch: false
tags: ["Ubuntu","VMware"]
categories: ["消灭bug的感觉无与伦比"]


lightgallery: true
---
虚拟机可以上网，主机浏览器不能
<!--more-->

## 项目场景：

使用VMware Workstation Pro安装ubantu虚拟机，NAT模式


## 问题描述：

虚拟机可以上网，主机浏览器不能


## 解决方案：
百度查到的解决方案，亲测可行
在Internet属性-连接-局域网设置中，三个选框都不选中，浏览器恢复正常


![solution](https://img-blog.csdnimg.cn/20210630104613874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)



