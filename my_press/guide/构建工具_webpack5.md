---
title: webpack5
category: 构建工具
tag:
  - webpack5
comment: false
---
# Webpack工作原理
1. 根据项目中webpack.config.js或shell语句中获取配置参数。
2. 将使用到的webpack插件实例化，在webpack事件流上挂载插件钩子
3. 根据配置所定义的入口文件进行依赖收集，对所有依赖的文件进行编译，编译过程依赖loaders，不同文件根据开发者定义的不同loader解析。编译好的内容使用acorn或其他抽象语法树能力解析成抽象语法树，分析文件依赖关系，将不同模块化语法替换为__webpack__require。
4. 结果产出到output


# Webpack打包CommonJS和ES_Module的区别
## 示例代码结构
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="app"></div>
  <script src="./dist/main.js"></script>
</body>
</html>
```
```javascript
// const sayHello = require("./hello");
import sayHello from "./hello";
console.log(sayHello("Tony"));
```

## CommonJS
```javascript
// CommonJS
/******/ (() => { // webpack函数立即调用表达式
/******/ 	var __webpack_modules__ = ({

/***/ "./src/hello.js":
/*!**********************!*\
  !*** ./src/hello.js ***!
  \**********************/
/***/ ((module) => {

eval("module.exports = function (name) {\r\n  return \"hello\" + name;\r\n};\r\n\n\n//# sourceURL=webpack://webpack-demo/./src/hello.js?");

/***/ }),

/***/ "./src/index.js":
/*!**********************!*\
  !*** ./src/index.js ***!
  \**********************/
/***/ ((__unused_webpack_module, __unused_webpack_exports, __webpack_require__) => {

eval("const sayHello = __webpack_require__(/*! ./hello */ \"./src/hello.js\");\r\nconsole.log(sayHello(\"Tony\"));\r\n\n\n//# sourceURL=webpack://webpack-demo/./src/index.js?");

/***/ })

/******/ 	});
/************************************************************************/
/******/ 	// 使用闭包将已经加载的模块保存在内存中
/******/ 	var __webpack_module_cache__ = {};
/******/ 	
/******/ 	// 定义模块加载函数
/******/ 	function __webpack_require__(moduleId) {
/******/ 		// 检查当前模块是否已缓存
/******/ 		var cachedModule = __webpack_module_cache__[moduleId];
/******/ 		if (cachedModule !== undefined) {
/******/ 			return cachedModule.exports;
/******/ 		}
/******/ 		// Create a new module (and put it into the cache)
/******/ 		var module = __webpack_module_cache__[moduleId] = {
/******/ 			// no module.id needed
/******/ 			// no module.loaded needed
/******/ 			exports: {}
/******/ 		};
/******/ 	
/******/ 		// 触发eval，会根据代码的依赖模块对exports进行赋值，入口文件是不会用到module和module.exports的
/******/ 		__webpack_modules__[moduleId](module, module.exports, __webpack_require__);
/******/ 	
/******/ 		// Return the exports of the module
/******/ 		return module.exports;
/******/ 	}
/******/ 	
/************************************************************************/
/******/ 	
/******/ 	// startup
/******/ 	// Load entry module and return exports
/******/ 	// This entry module can't be inlined because the eval devtool is used.
/******/ 	var __webpack_exports__ = __webpack_require__("./src/index.js");
/******/ 	
/******/ })();
```
## es_module
webpack对不同模块的打包结果整体的结构是类似的，es_module打包结果会多一个__webpack_require.r方法
```javascript
(() => {
/******/ 		// 对exports增加es模块化标记
/******/ 		__webpack_require__.r = (exports) => {
/******/ 			if(typeof Symbol !== 'undefined' && Symbol.toStringTag) {
                // 通过对exports增加Symbol.toStringTag属性，在调用toString()方法时会返回[object Module]
/******/ 				Object.defineProperty(exports, Symbol.toStringTag, { value: 'Module'   });
/******/ 			}
/******/ 			Object.defineProperty(exports, '__esModule', { value: true });
/******/ 		};
/******/ 	})();
/******/ 	
```

