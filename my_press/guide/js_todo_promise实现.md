---
title: Symbol使用
category: js
tag:
  - es6
comment: false
---

# Promise/A+规范
- 构造函数返回一个Promise实例
- 实例包括then方法，接收两个参数（onfulfilled和onjected）
    - onfulfilled可获取执行器函数触发resolve时的值
    - onjected时可获取执行器触发reject时的原因
- promise的状态只能从pending->fulfilled或者pending->rejected
```javascript
    let p = new Promise((resolve,reject)=>{
        resolve('resolve')
        reject('reject')
    })
    p.then((res)=>{
        console.log(res)
    },err=>{
        console.log(err)
    })
```
所以以上代码输出为resolve

## 暂停一下

我们基于以上三条规则实现的Promise的基本框架如下：
```javascript
class Promise {
  status = "pending";
  value = null; // 触发resolve时接收的值
  reason = null; // 触发reject时接收的值
  constructor(executor) {
    executor(this.resolve.bind(this), this.reject.bind(this)); // 需要通过bind修改this指向为当前实例
  }
  static resolve(value) {
    if (this.status === "pending") {
      this.value = value;
      this.status = "fulfilled";
    }
  }
  static reject(reason) {
    if (this.status === "pending") {
      this.reason = reason;
      this.status = "rejected";
    }
  }
  then(onfulfilled, onrejected) {
    if (this.status === "fulfilled") onfulfilled(this.value);
    if (this.status === "rejected") onrejected(this.value);
  }
}

let p = new Promise((resolve, reject) => {
  resolve("custom promise");
});
p.then(
  (res) => {
    console.log(res);
  },
  (err) => {
    console.log(err);
  }
);
```
> 这里我们需要注意，resolve和reject两个静态方法在执行器作为实参传递，实参传递实际上等同于变量赋值，也就是说我们将resolve/reject的引用地址赋值给执行器的形参，因此方法中的this是不符合预期的，我们可以使用bind绑定。

由以上代码我们可以看出代码是同步执行的

## todo 异步处理及链式处理补充，在学完宏任务微任务后补充