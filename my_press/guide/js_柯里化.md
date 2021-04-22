---
title: JS柯里化
category: js
tag:
  - 柯里化
comment: false
---
# 柯里化
> 维基百科：柯里化，英语：Currying(果然是满满的英译中的既视感)，是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。

直接看概念可能有些抽象，可以通过以下代码加深理解

```javascript
// 基本函数封装
const sum = (a, b, c)=>a + b + c
sum(1, 2, 3) // 6
// 柯里化后
const sum = a => b => c =>a + b + c
sum(1)(2)(3) // 6
```

## 柯里化的优点
### 1. 参数复用
```javascript
 const check = (reg, str)=>{
     return reg.test(str)
 }
 check(/\d+/g, 'test')       //false
 check(/[a-z]+/g, 'test')    //true
 const curryingCheck = (reg)=>{
     return str=>reg.test(str)
 }
 const hasNum = curryingCheck(/\d+/g)
 const hasLetter = curryingCheck(/[a-z]+/g)
 
 hasNumber('test1')      // true
 hasNumber('testtest')   // false
 hasLetter('21212')      // false
```
### 2. 提前确认
### 3. 延迟运行
```javascript
var curryWeight = function(fn) {
    var _fishWeight = [];
    return function() {
        if (arguments.length === 0) {
            return fn.apply(null, _fishWeight);
        } else {
            _fishWeight = _fishWeight.concat([].slice.call(arguments));
        }
    }
};
var fishWeight = 0;
var addWeight = curryWeight(function() {
    var i=0; len = arguments.length;
    for (i; i<len; i+=1) {
        fishWeight += arguments[i];
    }
});

addWeight(2.3);
addWeight(6.5);
addWeight(1.2);
addWeight(2.5);
addWeight();    //  这里计算

console.log(fishWeight);    // 12.5
```

## 柯里化的实现
```javascript
    const curry = (fn)=>{
        let args = [].slice.call(arguments, 1)
        return ()=>{
            args = args.concat([].slice.call(arguments))
            return fn.apply(args)
        }
    }
```

```javascript
Function.prototype.bind = function(context){
    return (...args)=>this.apply(context,args)
}
```