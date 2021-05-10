---
title: JS知识点汇总
category: js
tag:
  - js执行顺序
comment: false
---
# 前言
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

# 调用栈

## 概念
调用栈是用来管理函数调用关系的数据结构。

## 场景
在执行一段js代码时，javascript引擎会创建全局执行上下文（包含了声明的函数和变量）并压入栈结构，在执行声明的函数时，针对函数的作用域生成函数执行上下文，并继续压入调用栈，当函数执行完毕后执行上下文依次出栈。
![image](https://static001.geekbang.org/resource/image/7d/52/7d6c4c45db4ef9b900678092e6c53652.png)

我们可以在浏览器控制台Console区域通过call stack查看调用栈信息

## 栈溢出
调用栈是有大小的，当我们的调用栈到达了这个限制Maximum Call Stack时就会抛出错误信息。
例如：当我们的递归函数没有结束条件时

### 如何解决栈溢出问题呢
1. 首先我们可以通过异步的方式，将调用放在异步队列中（性能堪忧）
2. 可以通过[蹦床函数](https://es6.ruanyifeng.com/#docs/function)实现安全递归 
3. 也可以通过[tco函数](https://es6.ruanyifeng.com/#docs/function)实现安全递归