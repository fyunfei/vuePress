---
title: JS知识点汇总
category: js
tag:
  - js执行顺序
comment: false
---
 前言
```javascript
    show()
    console.log('text')
    var text = 'text'
    function show(){
        console.log('show')
    }
```
以上代码不难看出最后的输出结果，show -> undefined，我们都知道这是由于变量提升的原因导致的额，但是通常只知道变量提升但是并没有深入理解变量提升及变量提升的具体过程。

# 变量提升

说到变量提升我们不得不提两个概念，**声明**和**赋值**，例如：
```javascript
    var a = 0
```
我们可以把以上拆分成两部分
```javascript
    var a // 声明
    a = 0 // 赋值
```

## 概念
> 变量提升（Hoisting）被认为是， Javascript中执行上下文 （特别是创建和执行阶段）工作方式的一种认识

我们可以理解为变量提升是将声明过程移到代码的最前面，但是这并不是真正意义上的移到代码最前面，而是在代码编译阶段被Javascript引擎放到内存中。

注：js执行包括编译和执行两个阶段

## js执行过程
### 1. 编译
![](https://static001.geekbang.org/resource/image/06/13/0655d18ec347a95dfbf843969a921a13.png)
由图可知编译后生成执行上下文和可执行代码，后续的执行过程都是基于执行上下文进行的；
### 2. 执行
- 在执行上下文中查找对应的声明函数或者变量
- 进行赋值或者函数调用操作


