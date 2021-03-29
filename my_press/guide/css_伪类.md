---
title: 伪类和伪元素，傻傻分不清楚
category: CSS
tag:
  - css
  - 伪类
  - 伪元素
comment: false
---

## 伪类和伪元素
为什么css要引入伪元素和伪类的概念呢，以下是 [css2.1 Selectors](https://www.w3.org/TR/CSS2/selector.html#pseudo-elements) 章节中对伪类与伪元素的描述：

CSS introduces the concepts of pseudo-elements and pseudo-classes  to permit formatting based on information that lies outside the document tree.

翻译过来就是使用伪元素和伪类用来允许基于文档树之外的信息进行格式化。下面分别对伪类和伪元素进行解释：

- 伪类用于当已有元素处于的某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化的。比如说，当用户悬停在指定的元素时，我们可以通过:hover 来描述这个元素的状态。虽然它和普通的 css 类相似，可以为已有的元素添加样式，但是它只有处于 dom 树无法描述的状态下才能为元素添加样式，所以将其称为伪类。
- 伪元素用于创建一些不在文档树中的元素，并为其添加样式。比如说，我们可以通过:before 来在一个元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在文档树中。

我们可以通过以下示例加深理解：
```
// 场景一：假如我们要获取的是第一个li，并加上背景颜色
    <html>
        <ul>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
        </ul>
    </html>
    
    <style>
        li:first-child{
            background: rgba(255,255,0);
        }
    </style>

// 场景二：使用::after表示div的文本内容
    <html>
       <div></div>
    </html>
    
    <style>
        div::after{
            content: "内容测试";
        }
    </style>
```

通过以上两个场景的对比，我们不难分辨出伪类和伪元素之间的区别，场景一下获取到的元素目标是将要在文档树种获取的，而场景二的操作目标是创建一个元素用来表示当前元素的最后一个子元素，而并不是从文档树中获取。

而用::表示伪元素也是为了方便区分两种概念，时间上伪元素也可以使用:来表示，采用了一种向下兼容的方式。

# 总结
伪类和伪元素在实际开发中我们都是拿来即用的，对一些概念还是摸棱两可的，所以在空余时间还要多读文章，多看书来填补自己在一些基础概念上的知识缺失。
