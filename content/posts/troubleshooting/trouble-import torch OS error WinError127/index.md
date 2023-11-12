---

title: "import torch OS error WinError127"
date: 2021-08-16T10:09:58+08:00
draft: false
lastmod: 2021-09-19T10:09:58+08:00

author: "kliiu"
authorLink: "https://kliiu.github.io"
hiddenFromSearch: false
tags: ["GitHub"]
categories: ["消灭bug"]


lightgallery: true
---
import torch OS error WinError127
<!--more-->

# 报错
{{< admonition failure "Error" >}}
OSError: [WinError 127] 找不到指定的程序。 Error loading "C:\Users\k\anaconda3\envs\torch-gan\lib\site-packages\torch\lib\torch_python.dll" or one of its dependencies.
![在这里插入图片描述](https://img-blog.csdnimg.cn/bb5d52040ae44e6a8833d8f2f24cc227.png)
{{< /admonition >}}
# 解决
[升级python版本](https://zhuanlan.zhihu.com/p/138530259)
有效
