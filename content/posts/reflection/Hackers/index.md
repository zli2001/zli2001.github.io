---
title: "Hackers学习交流平台总结"
date: 2022-05-19T20:18:43+08:00
lastmod: 2022-05-19T20:18:43+08:00
draft: True
author: "zli2001"
authorLink: "https://zli2001.github.io"
description: ""

tags: ["Python","flask"]
categories: ["Reflection"]




lightgallery: true

---

基于flask的多用户博客系统：Hackers学习交流平台。

<!--more-->

花了10天左右的时间，断断续续地学习、改进、设计，终于完成了还不错的效果。

![index](hackers.png)

详细内容在整理后会发布到GitHub。

## 项目描述

基于flask的技术交流分享平台，支持博客、社区、资源、私信、后台管理等多种功能。

### 技术栈

- Python
- HTML+CSS+JS
- Redis
- MySQL

## GET√

1. 熟悉了基于**flask**框架软件的设计流程、实现细节，熟悉了模型、模板、视图（MTV）项目结构。熟悉了**Jinja2**模板渲染、前后端的**post**、**get**数据交换。
2. 熟悉了程序后端对数据库的操作，jetBrains的DataGrip好好用~
3. JavaScript：通过对此项目中前端页面与后端交互的设计、调试过程，熟悉了JS的原理与使用，了解了便捷的web数据交互方式**Ajax**。第一次知道原来浏览器可以调试js,添加断点。
4. 直接在html内嵌入的style在刷新页面后马上会生效，而css中更改的style则需要清除缓存才会生效，因为css是保存至浏览器缓存中的。
5. 大费周章地去实现了通过阿里云api下载文件的功能，然后在心满意足地关掉电脑时突然意识到，设置一个简单的超链接就可以达到效果。


