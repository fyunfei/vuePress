---
title: HTML标签知识点
category: HTML
tag:
  - HTML
  - 知识点
---

# link标签
- link元素是**空元素**，只包含属性
- 此元素只能存在于 head 部分，不过它可出现任何次数
- 标签定义文档与外部资源的关系（主要通过rel属性）
- 标签最常见的用途是链接样式表

> **空元素**：标签中没有内容被称为空元素

> 常见的空元素标签：br hr img input link meta

我们在实际使用中比较常用的就是通过link导入css文件和favion.ico，但是我们也可以使用@import来导入css

## @import（css） 和 link导入时的区别
1. 从语法层面分析，@import是css内部提供的引用规则；link是html提供的标签，但是link标签不仅可以加载样式，还可以加载其它类型的文档（favion.ico）。
2. 加载顺序区别。加载页面时，link 标签引入的 CSS 被同时加载；@import 引入的 CSS 将在页面加载完毕后被加载，所以在我们平时开发过程中要尽量减少@import的使用
3. 兼容性区别。@import 是 CSS2.1 才有的语法，IE5+ 才能识别；link 标签作为 HTML 元素，不存在兼容性问题。（注：这个兼容性目前可忽略不计，但是要了解）
4. DOM 可控性区别。可以通过 JS 操作 DOM ，插入 link 标签来改变样式；由于 DOM 方法是基于文档的，无法使用 @import 的方式插入样式。

# 行内元素
定义：HTML4 中，元素被分成两大类: inline （内联元素）与 block（块级元素）。一个行内元素只占据它对应标签的边框所包含的空
间。
常见行内元素有： a b span img strong sub sup button input label select textarea

# 块级元素
定义：块级元素占父级元素的整个宽度
常见块级元素：div,ul,ol,li,dl,dt,h1,h2,h3,p

# 两者区别
- **格式上**：默认情况下，行内元素不会以新行开始，而块级元素会新起一行。
- **内容上**：默认情况下，行内元素只能包含文本和其他行内元素。而块级元素可以包含行内元素和其他块级元素。
- **盒模型属性**：行内元素设置 width 无效，height 无效（可以设置 line-hei
     ght），设置 margin 和 padding 的上下不会对其他元素产生影响。