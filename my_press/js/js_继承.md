---
title: JS继承
category: js
tag:
  - 继承
  - 原型、原型链
comment: false
---

# 构造函数、原型和实例的关系：

每个构造函数都有一个原型对象(prototype)，原型有一个属性指回构造函数(constructor)，而实例有一个内部指针指向原型(`__proto__`)。

# constructor

在学习原型链继承时，看过一些文章，在重写了子类的原型对象后，需要将原型对象的 constructor 指回子类，但是并不知道究竟是什么作用，这里引用知乎大佬的回答

> constructor 其实没有什么用处，只是 JavaScript 语言设计的历史遗留物。由于 constructor 属性是可以变更的，所以未必真的指向对象的构造函数，只是一个提示。不过，从编程习惯上，我们应该尽量让对象的 constructor 指向其构造函数，以维持这个惯例。
>
> 引用自 https://www.zhihu.com/question/19951896/answer/13457869

## constructor 的作用

1. 可以判断元素类型

# 判断方法

## instanceof

instance 是基于原型链进行查找，如果原型链中存在构造函数则返回 true

```javascript
// 例一
let str = "string";
str instanceof String; // true
```

## isPrototypeOf

```javascript
function Person() {
  this.run = true;
}
function Human() {
  this.cry = true;
}
Person.prototype = new Human();
let p = new Person();
Person.prototype.isPrototypeOf(p); // true
Human.prototype.isPrototypeOf(p); // true
```

# 原型链继承

原型链继承示例代码见==isPrototypeOf==

## 存在的问题

**1. 原形中包含的引用值会在所有实例间共享**

```javascript
function Father() {
  this.sons = ["zs", "zz", "zl"];
}
function Person() {}
Person.prototype = new Father();

let p1 = new Person();
let p2 = new Person();
p1.sons.push("黑子");

p1.sons; // ['zs','zz','zl','黑子']
p2.sons; // ['zs','zz','zl','黑子']
```

**2. 子类在实例话时不能给父类的构造函数传参**

# 盗用构造函数

其实现原理是在子类的构造函数中调用父类的构造函数；它限制了父类属性的范围，解决了共享数据的问题，同时也解决了父类构造函数传参问题

```javascript
function Father() {
  this.sons = ["zs", "zz", "zl"];
}
function Person() {
  Father.call(this); // add
}

let p1 = new Person();
let p2 = new Person();
p1.sons.push("黑子");

p1.sons; // ['zs','zz','zl','黑子']
p2.sons; // ['zs','zz','zl']
```

## 存在的问题

1. 无法继承父类原型方法，所有需要继承的方法都要在构造函数中声明

# tips

以上两种和组合继承是无法继承静态属性的（可以通过原型链解决静态属性继承的问题）
