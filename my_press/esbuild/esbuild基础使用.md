---
title: esbuild基础使用
category: esbuild
tag:
  - esbuild基础
comment: false
---

# esbuild（Bundler）入门

[如何看待前端 bundler esbuild？](https://www.zhihu.com/question/394060026/answer/1216011300)

```bash
npm i esbuild
```

```bash
.\node_modules\.bin\esbuild.cmd app.jsx --bundle --outfile=out.js
```

esbuild 的运行方式有三种：命令行、js、go；

## API

esbuild 有两个重要的 API 供我们使用：**transform**、**build**。

- **transform**：当我们的运行环境下不支持文件系统时，使用这种方式。
- **build**：支持文件系统可以使用。

这里我们主要以支持文件系统的配置为例：

### build API

#### entryPoints

项目的入口文件。

#### outfile

经过打包后的输出文件，仅支持单入口文件，多个入口需要使用 outDir。

#### bundle

将入口文件的所有依赖文件打包到同一个内联文件下，这种方式会导致单个文件大小过大。

```javascript
// 打包配置
const esbuild = require("esbuild");
esbuild.buildSync({
  entryPoints: ["main.ts"],
  bundle: true,
  outfile: "dist/bundle .js",
});
```

```javascript
import { sum } from "./src/utils";
sum(1, 2);
```

```javascript
// 打包后的结果 bundle.js
// src/utils.ts
var sum = (a, b) => a + b;

// main.js
sum(1, 2);
```

如果不设置 bundle 配置则打包后的结果不会将相关依赖同时打包到 dist 目录下，下面会说明针对这种情况如何配置。

#### define

根据官网描述，这个配置的作用是使用常量表达式来替换全局标识，也就是说在我们的代码中存在全局标识时，我们可以通过这个配置来修改这个标识，查看如下示例：

```javascript
// 打包配置
const esbuild = require('esbuild')
esbuild.buildSync({
    entryPoints: ['main.ts'],
    bundle: true,
    outfile: 'dist/bundle .js'
+   define: {
+   	NEEDCONSOLE: true
+	}
})// 配置
```

```javascript
// main.js
+if (NEEDCONSOLE) {
+    console.log('NEEDCONSOLE was changed by esbuild configure');
+}
```

> 注：这里的全局标识与全局环境变量意义不同，笔者在使用这个配置的时候也尝试了很多次

注意，main.js 中增加的这部分代码，NEEDCONSOLE 并没有声明（通常情况下这样的写法是不符合规范的），但是运行打包后的文件 bundle.js 会打印代码块里的输出，define 命令生效了。

> Tips：个人认为在对这个配置还是直接使用环境变量好一些。
>
> Todo：该配置的具体使用还有待探索。

#### format

配置项：

- **iife** 生成立即调用代码
- **cjs** 生成符合 commonjs 规范代码
- **esm** 生成符合 es module 规范代码

#### minify

压缩代码配置

#### platform

默认情况下，esbuild 会将代码打包为可在浏览器环境下运行的代码，如果想让打包后的代码运行在 node 下，需要增加配置

```json
platform: 'node'
```

#### serve

与 devServer 类似，用来做 hotfix，但是不支持 hot reload 需要单独配置**Watch**

参数如下：

- port
- host
- servedir
- onRequest

#### splitting

代码分割，与 webpack 的 code-split 类似，在多个入口下如果存在相同引用，则会将引用部分封装到一个 chunk 文件下用来缓存，优化页面加载。配置如下：

```javascript
const esbuild = require('esbuild')
esbuild.buildSync({
    entryPoints: ['main.ts', 'sub.ts'],
+   bundle: true,
+   splitting: true,
+   format: 'esm',
    outdir: 'dist',
    define: {
        GMAIL: true
    },
})
```

> 注：仅在 esm 模式下代码分割配置生效

#### target

配置生成 javascript 的目标环境，示例如下：

```javascript
require("esbuild").buildSync({
  entryPoints: ["app.js"],
  target: ["es2020", "chrome58", "firefox57", "safari11", "edge16", "node12"],
  outfile: "out.js",
});
```

配置格式：

```javascript
es[N], chrome[N], edge[N], firefox[N], ios[N], node[N], safari[N];
```
