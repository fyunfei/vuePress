---
title: 函数节流防抖
category: js
tag:
  - 节流防抖
comment: false
---

# 节流
规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在同一个单位时间内某事件被触发多次，只有一次能生效。

## 实现
```javascript
function throttle(func, delay) {
  let last, deferTimer
  return function (args) {
      let now = Date.now();
      if (last && now < last + delay) {
          clearTimeout(deferTimer);
          deferTimer = setTimeout(()=>{
              last = now;
              func.apply(this, args);
          }, delay)
      } else {
          last = now;
          func.apply(this, args);
      }
  }
}
```

# 防抖
在事件被触发n秒后再执行回调函数，如果在这n秒内又被触发，则重新计时。
## 实现
```javascript
function debounce(func, delay) {
  return function (args) {
    //每次事件被触发，都会清除当前的timeer，然后重写设置超时调用
    clearTimeout(func.id);
    func.id = setTimeout(() => {
      func.call(this, args);
    }, delay);
  };
}
```