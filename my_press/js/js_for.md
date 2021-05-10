---
title: for of/for in/forEach在业务环境中如何选择
category: js
tag:
  - for循环
comment: false
---

# for of/for in/forEach的区别
## 前言
在做公司业务的时候使用到了forEach循环，不过思路不是很清晰为什么要用forEach循环，以及相应场景下这三种遍历方式我该如何做选择，所以读了一篇大佬[张鑫旭关于for..in/for...of的博客](https://www.zhangxinxu.com/wordpress/2018/08/for-in-es6-for-of/)，整理了如下笔记加深自己的理解：

---
## 正文
**for...of...**
不可以枚举对象、可迭代数组、可中断(break、return)、迭代字符串、迭代类数组对象
```javascript
const arr = [2,3,4] 
for(let val of arr){
    console.log(val) // 2,3,4
}
```

**for...in...**
可以枚举对象、迭代数组、迭代类数组
```javascript
const obj = {
    a:'testa',
    b:'testb',
    c:'testc'
}
const arr = [2,3,4] 
for(let key in obj){
    console.log(key) // a,b,c
}
for(let key in arr){
    console.log(key) // 0,1,2
}
```
可枚举出数组的原型对象以及数组本身的属性值
```javascript
const arr = [2,3,4] 
arr.test = 'this is a test'
Array.prototype.show = function(){}

for(let key in arr){
    console.log(key) // 0,1,2,test,show
}
```
在开发过程中，数组的原型方法show可能我们并不需要，这样就暴露除了问题（可以使用hasOwnProperty解决原型方法的问题），但是test属性仍然存在，所以在日常开发过程中尽量不要使用该方法去遍历数组，除非开发时确定当前数组中存在非index属性需要遍历出来时可以使用for..in。
```javascript
hasOwnProperty(prop):方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键prop）。
```

**forEach**

可以过滤数组中的test属性，但是无法使用break,return


**在遍历数组时for..of和forEach的选择**

判断当前业务环境是否需要索引，如果需要使用forEach，反之使用for..of