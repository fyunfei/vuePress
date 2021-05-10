---
title: vue源码解析（草稿）
category: vue
tag:
  - 源码
comment: false
---

# 实例化 Vue 都经过那些流程

## 初始化\_init 方法

```javascript
// /instanse/index.js
function Vue(options) {
  if (process.env.NODE_ENV !== "production" && !(this instanceof Vue)) {
    warn("Vue is a constructor and should be called with the `new` keyword");
  }
  this._init(options); // 在实例化时会经过调用初始化方法这一步，方法是在initMixin声明的
}
```

initMixin 的主要功能是：

1. 对用户的配置项与系统的配置项合并，若当前为组件实例则进行特殊处理，并赋值给\$options 属性
2. 增加系统配置项属性 \_self、\_renderProxy、\_isVue、\_uid 等
3. 初始化应用实例/组件实例生命周期 **initLifecycle**
4. 初始化**事件？** **initEvents**
5. 初始化渲染 **initRender** 声明渲染函数\_c（内部使用），$createElement（暴露给外部），渲染函数接受四个参数tag, data, children, normalizationType；同时对$attr，\$listener 声明响应式
6. **调用 Hook**，声明当前生命周期为 beforeCreated
7. 初始化注入 inject **initInjections**，逐层查找对应祖先组件是否提供了\_provide 属性并将属性值绑定在 result 对象中，若存在 default 则将 default 作为父组件注入结果返回
8. 初始化 state 状态，**initState**，这其中经过了 initProps，initMethods，initData，initComputed，initWatch，具体调用哪一个初始化方法是由用户的配置对象决定的，这里等下详细介绍
9. 初始化当前组件的 provide **initProvide**
10. **调用 Hook**，声明当前生命周期为 created，此时 Vue 实例创建完成。
11. 当前配置是否指定绑定元素，若指定了则将当前实例挂载\$mount 到对应元素上。

用代码表示如下：

```javascript
// 3
initLifecycle(vm);
// 4
initEvents(vm);
// 5
initRender(vm);
// 6
callHook(vm, "beforeCreate");
// 7
initInjections(vm);
// 8
initState(vm);
// 9
initProvide(vm);
// 10
callHook(vm, "created");
```

- initProps 初始化 props，并声明 props 为响应式
- initMethods
- initData

# Vue 是如何声明响应式的

Vue2.x 主要是使用 Object.defineProperty 实现双向数据绑定的

## 初始化 Data（initData）经过的步骤

- 在\$mount 时会将 data 处理成为一个函数 mergedInstanceDataFn，第一步会调用该函数获取用户绑定的属性值
- 判断当前属性是否和 methods 和 props 重名
- 将\_data 中的属性代理到 vm 上

```javascript
proxy(vm, `_data`, key);
function proxy(target, source, key) {
  sharedPropertyDefinition.get = function proxyGetter() {
    return this[sourceKey][key];
  };
  sharedPropertyDefinition.set = function proxySetter(val) {
    this[sourceKey][key] = val;
  };
  Object.defineProperty(target, key, sharedPropertyDefinition);
}
```

- 将数据变为可观察的（observer）

```javascript
function observer(value, asRootData){
...
  else if (
    shouldObserve &&
    !isServerRendering() &&
    (Array.isArray(value) || isPlainObject(value)) &&
    Object.isExtensible(value) &&
    !value._isVue
  ) {
      ob = new Observer(value); // 实例化Observer类
  }
```

```javascript
// Observer类
export class Observer {
  value: any;
  dep: Dep;
  vmCount: number; // number of vms that have this object as root $data

  constructor(value: any) {
    this.value = value;
    this.dep = new Dep();
    this.vmCount = 0;
    def(value, "__ob__", this);
    if (Array.isArray(value)) {
      if (hasProto) {
        protoAugment(value, arrayMethods);
      } else {
        copyAugment(value, arrayMethods, arrayKeys);
      }
      this.observeArray(value);
    } else {
      this.walk(value);
    }
  }

  /**
   * Walk through all properties and convert them into
   * getter/setters. This method should only be called when
   * value type is Object.
   */
  walk(obj: Object) {
    const keys = Object.keys(obj);
    for (let i = 0; i < keys.length; i++) {
      defineReactive(obj, keys[i]);
    }
  }

  /**
   * Observe a list of Array items.
   */
  observeArray(items: Array<any>) {
    for (let i = 0, l = items.length; i < l; i++) {
      observe(items[i]);
    }
  }
}
```

# Vue 的依赖收集问题

**依赖收集**的目的是收集当前在视图上使用的属性，当属性修改时触发视图更新

**流程**：

- 在页面初始渲染时进行一次收集，当渲染需要用到某一个属性时，会执行 Dep 的 depend 方法修改全局 Dep.target，该值为 Wathcer 实例用来触发视图更新
- 通过调用 Watcher 类的 addDep 方法来对 Dep.subs 增加依赖
- 当修改值的时候依次触发 Dep.notify 来遍历 subs 收集数组并通过 Watcher 类的 update 来触发视图的更新
