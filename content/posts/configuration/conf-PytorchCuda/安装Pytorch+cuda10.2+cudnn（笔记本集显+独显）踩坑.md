---

title: "安装Pytorch+cuda10.2+cudnn（笔记本集显+独显）踩坑"
date: 2021-07-09T10:09:58+08:00
draft: false
lastmod: 2021-07-09T10:09:58+08:00

author: "kliiu"
authorLink: "https://kliiu.github.io"
hiddenFromSearch: false
tags: ["Python","config","Pytorch","cuda"]
categories: ["Configuration"]


lightgallery: true
---
安装Pytorch+cuda10.2+cudnn（笔记本集显+独显）踩坑
<!--more-->



## 项目场景：

安装pytorch+cuda+cudnn遇到的离谱事件

## 问题描述：


```python
torch.cuda.device_count()#返回值一直为1
```
一直以为是自己版本安装错误，于是安完cuda11又卸载又安10.0，再卸载，再安10.2
最后还是1。

## 解决方案：
安了10.2，想随便找个测试，监测独显使用率。
结果如下，感觉自己好蠢。一开始就该get_device_name的。
记录一下。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709160858441.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgxNDcyOA==,size_16,color_FFFFFF,t_70)


测试代码转载自：
```html
https://blog.csdn.net/weixin_35576881/article/details/89709116`
```






