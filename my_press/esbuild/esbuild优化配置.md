---
title: esbuild优化配置
category: esbuild
tag:
  - esbuild优化
comment: false
---

# esbuild 配置优化

## Allow overwrite

```javascript
allowOverwrite: true;
```

该配置用来将构建后的文件覆盖入口文件，默认值为**false** 。

## Analyze

与 webpack 提供的 Analyze 功能类似，提供给我们一个表格用来查看 bundle 构建前后的对比，方便我们针对性的进行优化。

```javascript
(async () => {
  const esbuild = require("esbuild");
  let result = esbuild.buildSync({
    entryPoints: ["src/main.ts", "src/sub.ts"],
    bundle: true,
    splitting: true,
    metafile: true,
    outdir: "dist",
    define: {
      GMAIL: true,
    },
    format: "esm",
  });
  let text = await esbuild.analyzeMetafile(result.metafile);
  console.log(text);
})();
```

![image](http://image.followmyheart.cn/analyze%E6%89%93%E5%8D%B0%E7%BB%93%E6%9E%9C.png)

## AssetNames

```javascript
assetNames: 'assets/[dir]/[name]-[hash]',
loader: {".jpg": "file"}
```

当设置对应类型文件的 loader 为 file 时，在生成资源名称会根据配置的规则生成。

名称规则：

- **[dir]**
- **[name]**
- **[hash]**

## ChunkNames

用来控制对应 Chunk 的名称，用法与 AssetNames 相同。

## EntryNames

在多入口时用来控制入口文件的输出名称，用法与 AssetNames 相同。

## Banner

对应打包文件的 banner 部分的注释，不会替换掉文件构建是默认生成的路径注释。

```javascript
banner: {
	js: '/* This is js */',
	css: '/* This is css */'
},
```

## Footer

对应打包文件 Footer 部分的注释。

```javascript
footer: {
    js: '/* This is js */',
	css: '/* This is css */'
}
```

## Outbase

该配置说明输出文件是基于那一个基础目录下（通常情况下为入口文件的起始目录）的。配置如下：

```javascript
esbuild.buildSync({
        entryPoints: ['src/main.ts', 'src/sub.ts'],
        bundle: true,
        splitting: true,
        metafile: true,
        outdir: 'dist',
+       outbase: 'src',
        define: {
            GMAIL: true
        },
        format: 'esm',
        chunkNames: 'chunks/[name]-[hash]',
        entryNames: '[dir]/[name]-[hash]',
        banner: {
            js: '/* hahaha */',
            css: '/* This is css */'
        },
    })
```

当我们不使用 outbase 配置时，此时 dist 目录下有 src/main-[hash].js、src/sub-[hash].js 两个文件夹，如果我们想将入口文件放在 dist 最外层则可以增加**outbase: 'src'**。
