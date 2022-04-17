---
title: "How to show pictures in hugo blog"
subtitle: ""
date: 2022-04-17T11:21:46+08:00
lastmod: 2022-04-17T11:21:46+08:00
draft: false
author: "kliiu"
authorLink: "https://kliiu.github.io"
description: ""

tags: ["troubleshooting"]
categories: ["Blog"]

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: ""
featuredImagePreview: ""

toc:
  enable: true
math:
  enable: false
lightgallery: false
license: ""
---
If you can see your pictures in md file but not on hugo site, this is what you should do.
<!--more-->

{{< admonition note "Note" >}}
1. creat a folder in `/posts/` to save your blog.
2. creat a md file **named "index.md" or "index.en.md"** in this folder.
   put pictures in this folder and use them like this:
  ```markdown
  ![Liquid](Liquid.png "Liquid Exception")
  ```
{{< /admonition >}}


