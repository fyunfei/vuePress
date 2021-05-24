---
title: babel基础使用（一）
category: babel
tag:
  - babel基础
comment: false
---

# 一、 基础使用

Babel 是一个工具链，主要用于在旧的浏览器或环境中将 ECMAScript 2015+ 代码转换为向后兼容版本的

## @babel/core

## @babel/cli

这个依赖主要是对 babel 提供终端操作。

## babel 插件和预设

我们可以通过配置文件或者命令行来标明 babel 需要引入的插件(plugins)或预设(presets)，插件主要提供了针对单个语法的处理，预设则会根据我们代码中使用的语法进行 babel 插件的按需引入处理。

### 预设 Presets

babel 的预设我们可以理解为两部分，一部分是 syntax 层面的，另一方面是 api 层面的，我们可以查看一下例子

```javascript
// babel配置
{
  "presets": [
    [
      "@babel/env"
    ]
  ]
}
```

```javascript
const hasPerson = (name) => persons.includes(name);
// babel编译后
var hasPerson = function hasPerson(name) {
  return persons.includes(name);
};
```

在这个函数中我们使用了 ES6 提出的关于数组操作 includes 方法，经过编译后他并没有将 includes 转换为 es5 语法，这也就没有达到我们使用 babel 编译的目的，在 babel7 之前我们需要手动引入@babel/polyfill 用来达到 API 兼容的问题，但是在 babel7 我们可以直接通过配置去进行引入

```javascript
{
  "presets": [
    [
      "@babel/env",
      {
        "targets": {},
        "useBuiltIns": "usage",
        "corejs": "3.6.5"
      }
    ]
  ]
}
```

#### 配置说明

- targets 表示 Syntax/API 对浏览器的兼容，读取配置时，目录下.browserlist 文件的优先级大于 targets
- useBuiltIns 表示我们当前的引入方式，3 个可选值：**false**，**usage**，**entry**
  - usage 对语法 polyfill 的按需引入
  - entry 在文件中引入全部 polyfill
- corejs 这是一个 js 的标准库，包括针对 ES2015+的所有新特性 API 的 polyfill，默认值为 2.0.0 版本
