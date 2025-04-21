---
title: "2022泰迪杯总结"
subtitle: ""
date: 2022-04-30T22:18:43+08:00
lastmod: 2022-04-30T22:18:43+08:00
draft: false
author: "zli2001"
authorLink: "https://zli2001.github.io"
description: ""

tags: ["Python","data"]
categories: ["Reflection"]

hiddenFromHomePage: false
hiddenFromSearch: false



toc:
  enable: true
math:
  enable: false
lightgallery: false
license: ""
---

2022泰迪杯数据挖掘挑战赛C题总结

<!--more-->



## 经验与不足

1. 这次在处理问题时，将自己的思路清晰地记录了下来（见另一篇文章《2022泰迪杯数据挖掘挑战赛C题思路》），包括解题的过程、阅读的论文思路以及todo list。这样不仅做起来更有条理，也便于写论文，可谓是事半功倍。
2. 作为一个仍然是偏向建模类的比赛，论文是十分重要的，但是这一次我个人在论文上投入的时间显然还是不够，犯了以前每次都会犯的错，执着于花过量的时间在代码上，导致最后论文瑕疵还是比较多，也有许多图来不及画。当然，跟上一次泰迪杯相比还是有了很大的进步。
2. 这一次自己建立出了一个还不错的模型，虽然后面没有时间求解了，对自己的表现很满意。
4. 由于Jupyter notebook 不好调试，在调简单的代码上浪费了很多时间，以后应该换一个比较好调试的IDE。
4. 学会了写总结，非常的不错。（在总结里套娃）

## 技术

1. 关于pandas:

   - loc和iloc

   ```python
   # loc方法是按行索引标签选取数据：index的值
   df.loc[1,'正文']#选择index为1，'正文'列的数据
     
   
   # iloc方法是按行索引位置选取数据：行号
   
   df.iloc[0,1]#第0行第1列的数据
   ```

   - 更新了IPykernel以后出现了很多问题

     无法直接修改值了，如：`df.loc[1,'正文']="今天"`，会报错。只能重新创建一行并插入。

     

   - 转换为csv乱码可以，需要将编码格式设为：

     `task1_ans.to_csv('testresult1.csv',index=False,encoding='utf_8_sig')`
   
2. 了解了有趣的PageRank算法
3. 了解并使用HanLP这个十分强大的开源自然语言处理工具。
   
     
