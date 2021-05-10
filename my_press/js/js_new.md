---
title: new关键字
category: js
tag:
  - new
comment: false
---

# new 关键字做了哪些事

1. 创建一个空对象，将这个对象作为构造函数执行后返回的实例
2. 将创建的对象的原型指针指（`__proto__`）向构造函数的原型对象（prototype）
3. 将这个空对象赋值给构造函数内部 this，并执行构造函数逻辑
4. 返回第一步创建的对象（注：若构造函数有显示返回对象则创建后的实例为该对象）

实现 code：

```javascript
function newCustom(...args) {
  const constructor = args.shift();
  const obj = Object.create(constructor.prototype);
  const res = constructor.apply(obj, args);
  return typeof res === "object" && res !== null ? res : obj;
}
```
