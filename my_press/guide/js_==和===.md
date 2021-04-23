---
title: ==和===的区别
category: js
tag:
  - 宽松相等
  - 严格相等
comment: false
---
# 前言
宽松相等(==)和严格相等( === )： == 允许在相等比较中进行强制类型转换，而 === 不允许

# 相等比较操作的性能

# == 与 === 的比较规则
以下列举出的规则仅仅针对宽松相等(==)
## 非常规情况
- NaN 不等于 NaN
- +0等于-0

## 常规情况

### 字符串与数字之间比较
**eg:**
```
    let a = 42;
    let b = '42';
    a === b; // false
    a == b; // true
```

ES5规范：
1. 如果Type(a)是数字，Type(b)是字符串，则返回a == ToNumber(b)的结果
2. 如果Type(a)是字符串，Type(b)是数字，则返回ToNumber(a) == b的结果

### 其他类型和布尔类型之间比较
**eg**
```
    let a = 'test';
    let b = true
    a === b // false
    a == b // false
```
ES5规范:
1. 如果Type(a)是布尔类型，则返回ToNumber(a) == b的结果
2. 如果Type(b)是布尔类型，则返回a == ToNumber(b)的结果

### null 和 undefined比较
**eg**
```
    let a;
    a === undefined // true
    a === null // false
    a == undefined // true
    a == null // true
```

ES5规范：
1. 如果Type(a)是null，Type(b)是undefined，则返回true 
2. 如果Type(a)是undefined，Type(b)是null，则返回true

### 对象和非对象之间的比较
**eg**
```
    let a = 10
    let b = [10]
    a == b // true
    let obj = {valueOf:function(){
        return a
    }}
    a == obj // true
```
ES5规范：
1. 如果Type(a)是字符串或数字，Type(b)是对象，则返回a == ToPrimitive(b) 的结果。
2. 如果Type(a)是对象，Type(b)是字符串或数字，则返回 ToPrimitive(a) == b 的结果。

> 注：示例2中当字符串或数字和对象比较时，ToPrimitive() 主要时通过valueOf、toString打开封装对象拿到封装对象中的value

比较规则优先级：
> 注：无论什么条件，Boolean比较规则优先级最高

>     null/undefined > boolean  > object > string/number

eg:
```javascript
    // null/undefined优先级最高
    false == null // false
    false == undefined // false
    false == '0' 
    /* 经历过程：1. 根据boolean string规则转换为 0 == '0'
                 2. 根据number string 规则转换为 0 == 0
    */
    false == '' // true 
    /* 经历过程：1. boolean类型转换为number 0 -> 0 == ''
                 2. string类型转换为number 0 -> 0 == 0 true 
    */
    false == {} // false
    false == [] // false
    false == NaN // false 相当于string与number之间的比较
```