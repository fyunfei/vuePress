---
title: babel基础使用（一）
category: babel
tag:
  - babel基础
comment: false
---

# Babel
## 简介
Babel是一个用来帮我们转换javascript语法的工具，它具有向下兼容的特性，因此我们可以在开发中使用新的语法同时兼容主流浏览器。
## 开始使用
```bash
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```
- **@babel/core** babel核心功能模块
- **@babel/cli** babel终端命令工具
- **@babel/preset-env** 这个preset包括支持现代JavaScript（ES5+）的所有插件，可以通过配置文件进行按需引入配置

1. 在babel.config.js中写入配置
2. 运行以下命令将代码编译到lib目录
```bash
./node_modules/.bin/babel src --out-dir lib
```

## babel.config.json使用
在项目根目录创建babel.config.json文件，注：该文件不能为空

具体配置结构如下：
```javascript
    {
        "presets": [],
        "plugins": []
    }
```
也可以在package.json增加配置：
```javascript
    {
        "babel": {
            "presets": [],
            "plugins": []
        }
    }
```

## @babel/preset-env配置
这个插件提供给我们预设配置用来针对不同场景下的语法转换，注：语法支持不支持stage-3以下的版本，因为stage-3以下的版本语法浏览器不支持。

### Browserslist Integration
上面示例中我们进行了@babel/preset-env的targets配置去指定语法支持的浏览器版本，我们也可以使用==.browserslistrc==去指定兼容版本。

通常情况下，@babel/preset-env如果不存在targets或ignoreBrowserslistConfig配置时会使用[browserslist config sources](https://github.com/browserslist/browserslist#queries)

例如：.browserslistrc
```javascript
    > 0.25%
    not dead
```

### 配置
#### targets
#### bugfixes
这个配置生效的前提是需要配置targets，之后它会根据targets的配置对代码进行优化，请查看以下babel示例：

```javascript
// babel 配置文件
// 这里的配置告诉babel将语法转换为支持esmodules的语法
{
    "presets": [
        [
            "@babel/preset-env",
            {
                "targets": {
                    "esmodules": true
                }
            }
        ]
    ]
}
```

```javascript
// src/main.js 源文件
const foo = ({ a = 1 }, b = 2, ...args) => [a, b, args];
```

```javascript
// lib/mian.js 构建后的文件
var foo = function foo(_ref) {
  var {
    a = 1
  } = _ref;
  var b = arguments.length > 1 && arguments[1] !== undefined ? arguments[1] : 2;

  for (var _len = arguments.length, args = new Array(_len > 2 ? _len - 2 : 0), _key = 2; _key < _len; _key++) {
    args[_key - 2] = arguments[_key];
  }

  return [a, b, args];
};
```

这样我们就可以使用script标签加载esmodule，但是这样有可能会造成构建后的文件体积增大，所以这里就会用到bugfixes配置，结果如下：

```javascript
const foo = ({
  a: _a = 1
}, b = 2, ...args) => [_a, b, args];
```

显然通过bugfixes配置可以使代码压缩尽可能不增加文件体积，babel-v8这个配置默认为true

#### loose
“松散”配置，该配置用到的较少，当值为==false==时，生成的代码获得了最严格的，并与标准兼容，具有许多检查；当值为==true==时，生成的代码相对来说更轻量，遵循规格更小，但产生相同的结果。

上面的例子：
```javascript
const foo = ({ a = 1 }, b = 2, ...args) => [a, b, args];
```

```javascript
// loose：false
var foo = function foo(_ref) {
  var {
    a = 1
  } = _ref;
# var b = arguments.length > 1 && arguments[1] !== undefined ?  arguments[1] : 2;

  for (var _len = arguments.length, args = new Array(_len > 2 ? _len - 2 : 0), _key = 2; _key < _len; _key++) {
    args[_key - 2] = arguments[_key];
  }

  return [a, b, args];
};
```

```javascript
// loose：true
var foo = function foo(_ref, b) {
  var {
    a = 1
  } = _ref;

# if (b === void 0) {
#   b = 2;
# }

  for (var _len = arguments.length, args = new Array(_len > 2 ? _len - 2 : 0), _key = 2; _key < _len; _key++) {
    args[_key - 2] = arguments[_key];
  }

  return [a, b, args];
};
```

注意观察“#”标注代码，针对参数默认值的polyfill，loose配置为false的明显比true校验更加严格。

#### modules
模块配置，可选项为："amd" | "umd" | "systemjs" | "commonjs" | "cjs" | "auto" | false, 默认值"auto"，他可以根据我们的配置转换成不同规范下的代码。

#### include
用来引入插件配置，默认为[]

插件配置可以为如下两种：
- @babel/[custom]-[plugin] 或者不需要前缀的[custom]-[plugin]
- corejs 的内部模块，如es.map，es.set，

> 注：include、exclude仅支持preset包含的插件，由于Promise中不包含promise，所以在引入promise时会报错。

#### useBuiltIns
“usage”|“entry”|“false”默认值为false，它主要是针对我们在入口文件引用的corejs的相关配置，默认情况下，如果我们引入的是corejs，转换后的文件就是corejs，如果我们使用了entry配置，他会将corejs下包含的所有polyfill引入到入口文件中（通常情况下corejs中包括大量代码的polyfill，如果全部引入会造成单个文件体积过大的问题）；因此有了==usage==，这个配置会根据代码中用到的新特性，按需引入相关polyfill，因此大部分情况下我们会使用这个配置

> 注：使用useBuiltIns时需要选择corejs版本（也可以说Polyfill标准版本）