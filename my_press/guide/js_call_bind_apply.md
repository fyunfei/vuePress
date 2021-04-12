---
title: call、bind、apply
category: js
tag:
  - 改变this指向
  - 手写实现
comment: false
---
# 概念
call、bind、apply都是用来改变相关函数this指向的，但是call和apply直接进行相关函数调用，bind不会执行相关函数，而是返回一个新的函数，这个函数已经绑定了新的this指向

# bind函数基本实现
```javascript
Function.prototype.bind =
  Function.prototype.bind ||
  function (context) {
    let _this = this;
    let args = Array.prototype.slice.call(arguments, 1);
    return function () {
      // 获取内部函数参数，表示我们待绑定函数（目标函数）的参数
      let args_in = Array.prototype.slice.call(arguments);
      let final = args.concat(args_in);
      // 调用目标函数并绑定 this值
      return _this.apply(context, final);
    };
  };
```

> 注：这样实现的bind会存在以下问题:
> 1. 作为构造函数无法正确绑定this，我们也可以理解为new绑定比bind绑定的优先级要高，而我们目前实现bind在绑定构造函数时最后实例化是不会将参数初始化到实例上的。
> 2. 无法通过length去判断形参个数

针对第一点示例代码：
```javascript
function Person(name) {
  this.name = name;
}
let student = {
  name: "我是",
};
let worker = new (Person.bind(student))('王五');

console.log(worker); // {}

```

```javascript
// bind实现
Function.prototype.bind = Function.prototype.bind || function (context) {
  let _this = this;
  let args = Array.prototype.slice.call(arguments, 1);
  const F = function () {};
  F.prototype = _this.prototype;
  const bound = function () {
    let args_in = Array.prototype.slice.call(arguments);
    let final = args.concat(args_in);
    // 考虑构造函数存在返回值的情况
    let target = _this.apply(this instanceof F ? this : context || this, final);
    return typeof target === "object" && target !== null ? res : target;
  };
  bound.prototype = new F();
  return bound;
};
```
这种方案的实现原理是通过一个新的构造函数绑定当前构造函数的原型对象，并通过this instanceof F去判断当前是否为实例化操作，最后返回一个bound函数表示调用方法。