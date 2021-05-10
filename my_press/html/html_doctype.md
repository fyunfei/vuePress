---
title: DOCTYPE知识点
category: HTML
tag:
  - HTML
  - 知识点
comment: false
---

# 前言
在我们的开发过程中，经常会在html文件的第一行看到以下标签：
```
    <!DOCTYPE html>
```
那么它究竟有什么作用呢，接下来我就来介绍一下。

# DOCTYPE
我们平时解除的最多的就是HTML(超文本标记语言)，DOCTYPE的作用就是服务器标记语言的，它告诉了标记语言解析器要用什么文档类型定义来解析文档；同时DOCTYPE也决定了浏览器是使用**怪异模式**还是**标准模式**处理HTML文件。

## 怪异模式/标准模式的由来
> 在很久以前的网络上，页面通常有两种版本：为网景（Netscape）的 Navigator 准备的版本，以及为微软（Microsoft）的 Internet Explorer 准备的版本。当 W3C 创立网络标准后，为了不破坏当时既有的网站，浏览器不能直接弃用这些标准。因此，浏览器采用了两种模式，用以把能符合新规范的网站和老旧网站区分开。

## 怪异模式和标准模式的区别


## 浏览器如何判断当前渲染模式
### document.compatMode
- BackCompat为混杂模式
- Css1Compat为标准模式

