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

# hasOwnProperty
方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性

# 运算符
- ~ 按位非
- **>>** 右移
- << 左移

# 如何判断当前脚本运行在浏览器还是 node 环境中？
this === window ? 'browser' : 'node';

通过判断 Global 对象是否为 window，如果不为 window，当前脚本没有运行在浏览器中。

# Symbol 类型
1. Symbol 函数前不能使用 new 命令，否则会报错。
2. Symbol 函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。
3. Symbol 作为属性名，该属性不会出现在 for...in、for...of 循环中，也不会被 Object.keys()、Object.getOwnPropertyNames()、JSON.stringify() 返回。
4. Object.getOwnPropertySymbols 方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。
5. Symbol.for 接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建并返回一个以该字符串为名称的 Symbol 值。
6. Symbol.keyFor 方法返回一个已登记的 Symbol 类型值的 key。

# require 模块引入的查找方式
当 Node 遇到 require(X) 时，按下面的顺序处理。

> 1. 如果 X 是内置模块（比如 require('http')）
    - 返回该模块。
    - 不再继续执行。
> 2. 如果 X 以 "./" 或者 "/" 或者 "../" 开头
    - 根据 X 所在的父模块，确定 X 的绝对路径。
    - 将 X 当成文件，依次查找下面文件，只要其中有一个存在，就返回该文件，不再继续执行。
    
    X
    
    X.js
    
    X.json
    
    X.node
    - 将 X 当成目录，依次查找下面文件，只要其中有一个存在，就返回该文件，不再继续执行。
    
    X/package.json（main字段）
    
    X/index.js
    
    X/index.json
    
    X/index.node
> 3. 如果 X 不带路径
    - 根据 X 所在的父模块，确定 X 可能的安装目录。
    - 依次在每个目录中，将 X 当成文件名或目录名加载。
> 4. 抛出 "not found"