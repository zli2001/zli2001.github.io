---
title: "Hello Hugo"
date: 2022-03-19T15:06:09+08:00
draft: false

lastmod: 2022-04-15T10:09:58+08:00
draft: false
author: "kliiu"
authorLink: "https://kliiu.github.io"
description: "My first blog"
tags: ["hugo"]
categories: ["Blog"]

lightgallery: true
---
Hello World! Hello Hugo!

<!--more-->

### ~~Hugo发布步骤：~~

>--新建博客markdown文件，并编辑博客内容(文件名为 **.md )
>
> ``` hugo new post/useGit.md ```
> 
> ---生成静态页面
> 
> ```hugo --theme=dusky-neon-potato  --buildDrafts --baseUrl="https://kliiu.github.io/"```
> 
> ---发布
> 
> ```cd public ```
> 
> ```git add .```
> 
> ```git commit -m "new blog added"```
> 
> ```git push ```

### 改为部署在docs文件夹后的步骤

> ---打包
> 
> ```hugo -d docs```
> 
> ---push整个文件夹
> 
>  ...
> 