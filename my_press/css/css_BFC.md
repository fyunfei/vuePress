---
title: BFC
category: CSS
tag:
  - css
  - BFC
comment: false
---
# BFC（Block Formatting Content）的概念

是Web页面的可视CSS渲染的一部分，是块盒子（block-box）的布局过程发生的区域，也是浮动元素与其他元素交互的区域。

# 特性
- BFC 是一个独立的容器，容器内子元素不会影响容器外的元素。反之亦如此。
- 盒子从顶端开始垂直地一个接一个地排列，盒子之间垂直的间距是由 margin 决定的。
- 在同一个 BFC 中，两个相邻的块级盒子的垂直外边距会发生重叠。
- BFC 区域不会和 float box 发生重叠。
- BFC 能够识别并包含浮动元素，当计算其区域的高度时，浮动元素也可以参与计算了。

# 如何使用BFC
- 根元素默认为BFC
- float不为none的元素
- position为fixed或absoulute的元素
- display inline-block、flex、inline-flex、table-cell
- overflow hidden、scroll、auto

# BFC的应用
## 浮动元素导致父元素高度塌陷问题

## 外边距合并问题
> 外边距折叠（Margin collapsing）也只会发生在属于同一BFC的块级元素之间。 ----引用自MDN

### 解决方案
使用一个元素包裹其中一个元素并创建独立BFC区域
```
<style>
   body{
       overflow:hidden;
   }
   .child{
        width: 500px;
        height: 300px;
        margin: 20px;
        background-color: skyblue;
    }
    .child-1_con{
        overflow:hidden;
    }
</style>
<body>
    <div class="child child-1_con">
        <div class="child"></div>
    </div>
    <div class="child"></div>
</body>
```