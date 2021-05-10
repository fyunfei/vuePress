---
title: async、await原理及实战
category: js
tag:
  - es7
  - async、await
comment: false
---
# 前言
async，await的提出解决了Promise的大段链式调用问题，代码以同步的方式去写异步操作。
# 实现原理
promise+generator，使用Generator引入协程的概念

**async** 隐式返回一个promise

**await** 内部执行过程中创建一个promise并将后续的任务添加到微任务队列中，基于Promise.resolve，同时切换主线程控制权为主协程，当宏任务执行完后，在执行微任务队列中的resolve时将主线程的控制权交由async协程

# 输出结果实战
```javascript
async  function  async1 ()  {
    console.log('async1 start');
    await  async2();
    console.log('async1 end')
}
async  function  async2 ()  {
    console.log('async2')
}
console.log('script start');
setTimeout(function ()  {
    console.log('setTimeout')
},  0);
async1();
new  Promise(function (resolve)  {
    console.log('promise1');
    resolve()
}).then(function ()  {
    console.log('promise2')
});
console.log('script end')
```

## 流程分析
1. 声明 async1 async2函数
2. ==打印== script start
3. 遇到setTimeout定时任务的callback添加到延迟任务队列中，此时delayQueue = [console.log('setTimeout')]
4. 执行async1函数
    1. ==打印==async1 start
    2. 执行await async()，会先执行async2，==打印==async2
    3. await async执行完毕将主线程控制权交由主协程，同时将await下面的任务添加到微任务中，此时微任务mincrotask = [console.log('async1 end')]
5. 遇到promise打印promise1，resolve执行并将链式任务添加到微任务队列，此时mincrotask = [console.log(promise2),console.log('async1 end')]
6. ==打印==script end
7. 此时任务队列中宏任务执行完毕，继续执行微任务，根据队列的先进先出规则依次==打印==
8. async1 end
9. promise2
10. 此时微任务执行完毕，延迟任务队列中存在到期任务，==打印==setTimeout

**最终结果**：
1. script start
2. -> async1 start
3. -> async2
4. -> script end
5. -> async1 end 
6. -> promise2 
7. -> setTimeout

通过这个案例我们也可以发现，宏任务和微任务出现耗时过久的任务时会阻塞全局settimout的执行