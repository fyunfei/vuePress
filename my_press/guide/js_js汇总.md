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