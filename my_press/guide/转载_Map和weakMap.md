---
title: Map 与 WeakMap
category: 转载
tag:
  - 转载
  - Map 与 WeakMap
comment: false
---
# 【ES6 基础】Map 与 WeakMap

![](https://ss.csdn.net/p?https://mmbiz.qpic.cn/mmbiz_png/KEXUm19zKo6iaBnEoia2QCfncYrGLa8dZ0NHfGCg4Zx3vOGSXYrQWa5JicDVG8wqVWsLmicfdAOtUwzstrE2uQibyaw/640?wx_fmt=png)

## 开篇

ES6 里除了增加了 Set（集合）类型外（笔者在这篇文章[《Set 与 WeakSet》](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247484091&idx=1&sn=0cc25a65f9d4fed51f0f08198aecd375&chksm=a6a5110791d29811ab64302631a5df038c42843cfaefd5157155b6a3f0028b6c828c245c4a2a&scene=21#wechat_redirect)有过介绍），今天的这篇文章笔者将继续介绍 ES6 引入的新类型——Map(映射类型)和其对应的弱类型 WeakMap。映射类型在计算机科学中定义属于关联数组，而关联数组的定义是若干键值对 (Key/Value Pair) 组成的集合, 其中每个 Key 值都只能出现一次。

本篇文章将从以下方面进行介绍：

- Map 代码示例
- Map 常用方法示例
- Map 与 Object 的区别
- weakMap 介绍

## **01**

## **Map 代码示例**

Map 的键和值可以是任何数据类型，键值对按照插入顺序排列，下段代码演示了 Map 的一些用法：

```javascript
let map = new Map();
let o = { n: 1 };
map.set(o, "A"); //add
map.set("2", 9);
console.log(map.has("2")); //check if key exists
console.log(map.get(o)); //retrieve value associated with key
console.log(...map);
console.log(map);
map.delete("2"); //delete key and associated value
map.clear(); //delete everything
//create a map from iterable object
let map_1 = new Map([
  [1, 2],
  [4, 5],
]);
console.log(map_1.size); //number of keys
```

上述代码将会输出：

```javascript
true
A
[ { n: 1 }, 'A' ] [ '2', 9 ]
Map { { n: 1 } => 'A', '2' => 9 }
2
```

从上述代码中，我们可以看出使用 new Map() 语法进行声明，Map 键的类型可以使用任意对象作为键（字符串，object 类型），我们还可以直接以二维数组键值对的形传入到构建函数中，第一项为键，后一项为值。

Map 中如果插入重复的键，会怎么样？如下段代码所示：

```javascript
const map = new Map([
  ["foo", 1],
  ["foo", 2],
]);
console.log(map);
console.log(map.get("foo"));
```

上述代码将会输出：

```javascript
Map { 'foo' => 2 }
2
```

上述代码我们可以看出，如果存在相同的键，则会按照 FIFO（First in First Out, 先进先出）原则，后面的键值信息会覆盖前面的键值信息。

## **02**

## **Map 常用方法示例**

以下表格罗列了 Map 相关的常用操作方法：

| 操作方法           | 内容描述                                     |
| :----------------- | :------------------------------------------- |
| map.set(key,value) | 添加键值对到映射中                           |
| map.get(key)       | 获取映射中某一个键的对应值                   |
| map.delete(key)    | 将某一键值对移除映射                         |
| map.clear()        | 清空映射中所有键值对                         |
| map.entries()      | 返回一个以二元数组（键值对）作为元素的数组   |
| map.has(key)       | 检查映射中是否包含某一键值对                 |
| map.keys()         | 返回一个当前映射中所有键作为元素的可迭代对象 |
| map.values()       | 返回一个当前映射中所有值作为元素的可迭代对象 |
| map.size           | 映射中键值对的数量                           |

**增删键值对与清空 MAP**

```javascript
let user = { name: "Aaron", id: 1234 };
let userHobbyMap = new Map();
userHobbyMap.set(user, ["Ice fishing", "Family Outting"]); //添加键值对
console.log(userHobbyMap);
userHobbyMap.delete(user); //删除键值对
userHobbyMap.clear(); //清空键值对
console.log(userHobbyMap);
```

上述代码将会输出：

```javascript
Map { { name: 'Aaron', id: 1234 } => [ 'Ice fishing', 'Family Outting' ] }
Map {}
```

**获取键值对**

与 Set 集合对象不一样，集合对象的元素没有元素位置的标识，故没有办法获取集合某元素，但是映射对象由键值对组成，所以可以利用键来获取对应的值。

```js
const map = new Map();
map.set("foo", "bar");
console.log(map.get("foo")); //output bar
```

**检查映射对象中是否存在某键**

与 Set 集合一样，Map 映射也可以使用 has(键) 的方法来检查是否包含某键。

```javascript
const map = new Map([["foo", 1]]);
console.log(map.has("foo")); //output true
console.log(map.has("bar")); //output false
```

**遍历映射中的键值对**

映射对象在设计上同样也是一种可迭代的对象，可以通过 for-of 循环对其遍历，同时也可以使用 foreach 进行遍历。

映射对象中带有 entries() 方法，用于返回包含所有键值对的可迭代的二元数组对象，而 for-of 和 foreach 便是先利用 entries() 方法先将映射对象转换成一个类数组对象，然年再进行迭代。

```javascript
const map = new Map([
  ["foo", 1],
  ["bar", 2],
]);
console.log(Array.from(map.entries()));
//output
//[ [ 'foo', 1 ], [ 'bar', 2 ] ]
for (const [key, value] of map) {
  console.log(`${key}:${value}`);
}
//output
//foo:1
//bar:2
map.forEach((value, key, map) => console.log(`${key}:${value}`));
//output
//foo:1
//bar:2
```

## **03**

## **Map 与 Object 的区别**

说了这么多映射对象的方法，Map 和 Object 对象有哪些区别呢？以下表格进行了总结：

| 对比项                       | 映射对象 Map | Object 对象 |
| :--------------------------- | :----------: | :---------: |
| 存储键值对                   |      √       |      √      |
| 遍历所有的键值对             |      √       |      √      |
| 检查是否包含指定的键值对     |      √       |      √      |
| 使用字符串作为键             |      √       |      √      |
| 使用 Symbol 作为键           |      √       |      √      |
| 使用任意对象作为键           |      √       |
| 可以很方便的得知键值对的数量 |      √       |

从中我们可以看出 Map 对象可以使用任何对象作为键，这就解决了我们实际应用中一个很大的痛点，比如现在有一个 DOM 对象作为键时，Object 就不是那么好用了。

## **04**

## **WeakMap**

与集合类型（Set）一样，映射类型也有一个 Weak 版本的 WeakMap。WeakMap 和 WeakSet 很相似，只不过 WeakMap 的键会检查变量的引用，只要其中任意一个引用被释放，该键值对就会被删除。

以下三点是 Map 和 WeakMap 的主要区别：

1.  Map 对象的键可以是任何类型，但 WeakMap 对象中的键只能是对象引用
2.  WeakMap 不能包含无引用的对象，否则会被自动清除出集合（垃圾回收机制）。
3.  WeakSet 对象是不可枚举的，无法获取大小。

下段代码示例验证了 WeakMap 的以上特性：

```javascript
let weakmap = new WeakMap();
(function () {
  let o = { n: 1 };
  weakmap.set(o, "A");
})(); // here 'o' key is garbage collected
let s = { m: 1 };
weakmap.set(s, "B");
console.log(weakmap.get(s));
console.log(...weakmap); // exception thrown
weakmap.delete(s);
weakmap.clear(); // Exception, no such function
let weakmap_1 = new WeakMap([
  [{}, 2],
  [{}, 5],
]); //this works
console.log(weakmap_1.size); //undefined”
```

```javascript
const weakmap = new WeakMap();
let keyObject = { id: 1 };
const valObject = { score: 100 };
weakmap.set(keyObject, valObject);
console.log(weakmap.get(keyObject));
//output { score: 100 }
keyObject = null;
console.log(weakmap.has(keyObject));
//output false
```

## **05**

## **小节**

今天的内容就介绍到这里，我们明白了 Map 是一个键值对的映射对象，相比 Object 来说可以使用任何键做为键值，并且能够很方便的获取键值对。WeakMap 相对于 Map 是一个不可枚举的对象，必须使用对象作为键值。如何更好的使用 Map 和 WeakMap 还需要具体结合我们实际的业务场景进行灵活使用。
