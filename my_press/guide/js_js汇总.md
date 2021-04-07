---
title: JS知识点汇总
category: js
tag:
  - 汇总
comment: false
---
# 数据类型
包括基本数据类型和引用数据类型
- 基本数据类型（栈结构）：Number、Boolean、String、undefine、null、Symbol、BigInt
- 引用数据类型（堆结构）：Array、Object

## tips
- 可以使用void关键字获取安全undefined的值
- typeof null的结果是object，这其实是javascript语言的bug，没有修改是为了兼容旧代码

# 假值对象
浏览器在某些特定情况下，在常规 JavaScript 语法基础上自己创建了一些外来值，这些就是“假值对象”。假值对象看起来和
普通对象并无二致（都有属性，等等），但将它们强制类型转换为布尔值时结果为 false 最常见的例子是 document.all，它
是一个类数组对象，包含了页面上的所有元素，由 DOM（而不是 JavaScript 引擎）提供给 JavaScript 程序使用。