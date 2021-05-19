---
title: JS类型转换
category: js
tag:
  - 类型转换
comment: false
---

# JS数据类型及其转换
[ES规范](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Language_Resources)
## 前言
在看书时发现作者在柯里化功能的实现上使用了如下方式：
```javascript
 // add(2)(3)(4)
 const add = (...args)=>{
     const fn = (...args2)=>{
         args = [...args,...args2]
         return fn
     }
     fn.toString = ()=>{
        return args.reduce((total,item)=>total+item,0)
     }
     return fn
 }
```
这里不理解的一点是为什么要声明一个看似与需求无关的toString方法呢，通过学习了解到这里包括一层隐式调用。
> 注：这篇文章需要首先了解基础的类型转换规律

## 出现隐式调用的原因
我们都知道javascript是弱类型语言，这也就意味着变量的类型不需要我们提前声明，他会在运行过程中确定，因此就会出现如下情况：
```
1 + '1' // '11'
1 + true // 2
true + '' // 'true'
...
```
这也就是我们熟悉的隐式类型转换，但是请看如下情况：
```javascript
const obj = {
    name: 'zs'
}
1 + obj // '1[Object object]' 
+obj // NaN
```
通过查看ES规范，我们可以知道，在进行隐式转换过程中内部会调用toPrimitive

### toPrimitive

- 当示意转换的类型为string时，我们会先调用内部的toString方法，然后再调用valueOf方法
- 如果没有示意转换的类型时，我们会先调用valueOf方法，在调用toString方法

由于是在内部调用，属于隐式类型转换，说明我们并没有示意当前的转换类型，因此以上的类型转换步骤为：
```javascript
1 + obj
// 1. 先调用obj的valueOf方法获取到对象实例
// 2. 再调用toString方法来将实例转换成字符串 '[Object object]'
// 3. 就是进行简单的string + number的计算了 
```

当我们的操作没有涉及到隐式类型转换时，会优先调用toString：
```javascript
const obj = {
    toString(){
        return 'age'
    }
}
const person = {
    name:'zs',
    age: 15
}
person[obj] // 15
```
