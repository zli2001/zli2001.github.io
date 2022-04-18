---

title: "Error and Exception"
date: 2022-04-18T10:09:58+08:00
draft: false
lastmod: 2022-04-18T10:09:58+08:00

author: "kliiu"
authorLink: "https://kliiu.github.io"
description: "Compare error and exception."
hiddenFromSearch: false
tags: ["java","code"]
categories: ["Coding"]


lightgallery: true
---

<!--more-->

## Exception:

程序正常运行中可以预料的意外情况，并且应该被捕获进行处理。

- checked 必须显示进行捕获

  > IOException

- unchecked  根据需要判断是否需要捕获

  > NullPointerExceotion,ArrayIndexOutOfBoundsExceptin

## Error

正常情况下不大可能出现的情况，不需要捕获。

- LinkageError,NoClassDefFoundError
- VirtualMachineError,OutOfMemoryError,StackOverflowError

## 相同点

都继承自*Throwable*类，Java中只有*Throwable*类型的实例才可以被抛出（throw）或者捕获（catch）

参考资料：

[https://blog.csdn.net/m0_37602175/article/details/80271647](https://blog.csdn.net/m0_37602175/article/details/80271647)