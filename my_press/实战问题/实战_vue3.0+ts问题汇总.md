---
title: vue3.0+ts实战问题汇总
category: 实战
tag:
  - vue3.0
  - typescript
comment: false
---

# vite搭建最初项目框架
```
npm init @vitejs/app my-vue3 --template vue-ts
```
# 引入vue-router-next

[vue-route-next](https://next.router.vuejs.org)

# 修改package.json中vuedx/typecheck的版本为^0.4.2-insiders-1610169628.0解决构建时报错的问题

# npm i

# 安装babel
```
npm install --save-dev @babel/core @babel/cli @babel/preset-env
npm install --save @babel/polyfill
```

# 安装rollup babel插件
```
npm install @rollup/plugin-babel --save-dev
```
vite.config.js

```javascript
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import babel from "@rollup/plugin-babel";

export default defineConfig({
  plugins: [vue(), babel()],
});
```

# 引入babel-plugin-transform-remove-console，在生产环境去除console

```
npm install babel-plugin-transform-remove-console --save-dev
```

babel.config.js

```javascript
const plugins = []

if(process.env.NODE_ENV === 'production') plugins.push('transform-remove-console')

module.exports = {
  plugins
}
```

# 配置shims.d.ts解决引入*.vue文件报错但可正常编译的问题

```javascript
declare module "*.vue" {
  import { ComponentOptions } from "vue";
  const componentOptions: ComponentOptions;
  export default componentOptions;
}
```


# sass支持
```javascript
npm i sass, sass-loader, node-sass --save-dev
```

# 20210108新增
- vite配置深度引入模块时，脚手架安装的vite beta12报错，需要升级到beta29
[vitejs/vite/issues/1538](https://github.com/vitejs/vite/issues/1538)
- element-plus国际化配置问题，[issues#1310](https://github.com/element-plus/element-plus/issues/1310)

# 20210121新增
- element-plus按需引入需要获取到lib目录下的指定模块才可以，从主模块引用组件模块再打包时无效（注：引入lib库下的模块需要再vite.config.js中进行深度引用配置才可以，由于不支持通配符需要在引用相同目录下的不同模块需要写多个路径）
- ts文件中使用nodejs模块需要安装依赖@types/node

# 20210122新增
- 解决部分依赖（例如qs）模块采用Commonjs导致vite无法正确引用的问题
    1. 首先需要将目标依赖作为生产环境依赖
    2. 在vite的optimizeDeps中配置
    3. 安装类型补丁@types/qs
    4. [issues#986](https://github.com/vitejs/vite/issues/986)
    5. [issues#826](https://github.com/vitejs/vite/issues/826#issuecomment-713262444)

# 20210126
有时在我们的业务逻辑代码中引入其他工具方法时，使用vite配置的别名无效（例如：@ 表示src）
eg: 在 data.ts 中引入 @/request.ts 抛出的类 request时，IDE会提示我们“无法找到模块”，这需要我们在tsconig.json中配置别名才可以解决

```javascript

    {
        "compilerOptions":{
            "baseUrl": "./",
            "paths": {
                "@/*": ["src/*"]
            }
        }
    }

```

purgecss解决element-plus及font-awsome引入了未用到的css样式问题
> npm i @fullhuman/postcss-purgecss --save-dev

```javascript
// postcss.config.js
const purgecss = require('@fullhuman/postcss-purgecss')

module.exports = {
    plugins: [
        purgecss({
            content: ['./*.html', './src/**/*.vue']
        })
    ]
}

```

# 20210202
## __VITE_ASSETS__ 生产环境报错
[issue](https://github.com/vitejs/vite/issues/1602)


开发构建版本：vite2.0.0-beta.56、vitejs/compiler-sfc1.0.4