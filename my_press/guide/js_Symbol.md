---
title: Symbol使用
category: js
tag:
  - es6
comment: false
---

ES6 引入了一种新的原始数据类型**Symbol**，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

# 基础
```javascript
let s = Symbol();

typeof s // Symbol
```

上面代码中，变量s就是一个独一无二的值。typeof运算符的结果，表明变量s是 Symbol 数据类型，而不是字符串之类的其他类型。

> 不可以使用 new 关键字实例化

- 接受一个参数，这个参数表示描述，当参数是对象时，会调用对象的toString()方法作为描述
- Symbol可以转为布尔值，无法做字符运算


## Symbol.prototype.description
获取描述信息
```javascript
let s = Symbol('test')
s.description // test
```

## Symbol.for 
在全局查找是否已经被登记的Symbol，如果存在则返回，不存在则创建并登记Symbol
```javascript
let s = Symbol.for('mark') // 第一次登记
Symbol.for('mark') // Symbol(mark) 
```
- **与Symbol的区别？** Symbol不被全局登记可无限创建
- Symbol.for的登记范围不受作用域的影响

```javascript
    function t(){
        return Symbol.for('scoped')
    }
    let s = t()
    let q = Symbol.for(scoped)
    s === q // true
```

## Symbol.keyFor
获取当前已登记Symbol的描述

```javascript
let s = Symbol.for('keyFor')
Symbol.keyFor(s) // keyFor
```

## Symbol.hasInstance
对象的Symbol.hasInstance属性，指向一个内部方法。当其他对象使用instanceof运算符，判断是否为该对象的实例时，会调用这个方法。

```javascript
    class Person {
        name = '姓名';
        static [Symbol.hasInstance](foo) {
            console.log('instance test')
            return foo instanceof Person
        }
    }
    let p = new Person()
    p instanceof Person // true 'instance test'
```

## Symbol.isConcatSpreadable
对象的Symbol.isConcatSpreadable属性等于一个布尔值，表示该对象用于Array.prototype.concat()时，是否可以展开。

```javascript
// 直接作用在实例上
let arr1 = ['c', 'd'];
['a', 'b'].concat(arr1, 'e') // ['a', 'b', 'c', 'd', 'e']
arr1[Symbol.isConcatSpreadable] // undefined

let arr2 = ['c', 'd'];
arr2[Symbol.isConcatSpreadable] = false;
['a', 'b'].concat(arr2, 'e') // ['a', 'b', ['c','d'], 'e']
```

```javascript
// 作用在类上
class A1 extends Array {
  constructor(args) {
    super(args);
    // 构造函数赋值
    this[Symbol.isConcatSpreadable] = true;
  }
}
class A2 extends Array {
  constructor(args) {
    super(args);
  }
  // get赋值
  get [Symbol.isConcatSpreadable] () {
    return false;
  }
}
```

## Symbol.species
在创建衍生对象时会用到该属性，例如，我们通过数组的一些方法（filter、slice...）获得到新的数组时，这个新数组就表示衍生对象。
```javascript
class MyArray extends Array {
  static get [Symbol.species]() { return Array; }
}

const a = new MyArray();
const b = a.map(x => x);

b instanceof MyArray // false
b instanceof Array // true
```

## Symbol.match
对象的Symbol.match属性，指向一个函数。当执行str.match(myObject)时，如果该属性存在，会调用它，返回该方法的返回值。
```javascript
String.prototype.match(regexp)
// 等同于
regexp[Symbol.match](this)

class MyMatcher {
  [Symbol.match](string) {
    return 'hello world'.indexOf(string);
  }
}
'e'.match(new MyMatcher()) // 1
```
## Symbol.replace
当该对象被String.prototype.replace方法调用时，会返回该方法的返回值。
```javascript
const x = {};
x[Symbol.replace] = (...s) => console.log(s);

'Hello'.replace(x, 'World') // ["Hello", "World"]
```

类似功能还有 split、search，都是针对字符串的操作，用法与之相似

## Symbol.toPrimitive
在出现隐式类型转换时会触发该方法
```javascript
class Person{
    [Symbol.isPrimitive](hint){
        switch(hint){
          case 'number':
            return 123;
          case 'string':
            return 'str';
          case 'default':
            return 'default';
          default:
            throw new Error();
        }
    }
}

```

## Symbol.toStringTag
当我们调用Object.prototype.toString()时，会触发该函数并返回结果

```javascript
class Human{
    get [Symbol.toStringTag](){
        return '人类'
    }
}
let h = new Human()
Object.prototype.toString.call(h) // [object 人类]
```
