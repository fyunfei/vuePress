---
title: 实现reduce、compose
category: js
tag:
  - reduce
  - compose
comment: false
---
# reduce
> 对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值。

## 语法
> arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])

这里的callback我们可以理解为reducer
- **accumulator** 表示累加器的意思，用来记录上一次的返回结果
- **curreValue** 表示遍历中正在处理的元素
- **index** 表示处理元素的索引，初始时若没有initialValue，则索引为1，否则为0
- **array** 表示调用reduce的数组

**initialValue** 表示第一次调用的初始值


## 实现
```javascript
  const arr = this;
  let res = typeof initValue === "undefined" ? arr[0] : initValue;
  let i = typeof initValue === "undefined" ? 1 : 0;
  arr.slice(i).forEach((value, index) => {
    res = cb(res, value, index + i, arr);
  });
  return res;
```

# compose （待补充）