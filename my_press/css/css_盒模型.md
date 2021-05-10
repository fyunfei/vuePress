---
title: 伪类和伪元素，傻傻分不清楚
category: CSS
tag:
  - css
  - 盒模型
comment: false
---

# CSS盒子模型
## 盒模类型
1. IE盒模型（border-box）、w3c标准盒模型（content-box）

## 盒模型内容
![](https://media.prod.mdn.mozit.cloud/attachments/2014/09/29/8685/0930aa361239ef7b22a8da798410a855/boxmodel-(3).png)

由图可知，每个盒子分为四个区域，包括：Content、Padding、Border、Margin

### Content（内容区域）
他表示元素内部的“真实”内容，例如文字、图片等...，内容区域的大小可明确地通过width、min-width、max-width、height、min-height，和 max-height 控制。

### Padding（内边距区域）
> 由内边距边界限制，扩展自内容区域，负责==延伸内容区域==的背景，填充元素中内容与边框的间距。它的尺寸是 padding-box 宽度 和 padding-box 高度。

内边距的粗细可以由 padding-top、padding-right、padding-bottom、padding-left，和简写属性 padding 控制。

### Border（边框区域）
> 由边框边界限制，扩展自内边距区域，是容纳边框的区域。其尺寸为 border-box  宽度 和 border-box 高度。

边框的粗细由 border-width 和简写的 border 属性控制。如果 box-sizing 属性被设为 border-box，那么边框区域的大小可明确地通过 width、min-width, max-width、height、min-height，和 max-height 属性控制。假如框盒上设有背景（background-color 或 background-image），背景将会一直延伸至边框的外沿（默认为在边框下层延伸，边框会盖在背景上）。此默认表现可通过 CSS 属性 background-clip 来改变。


### Margin（外边距区域）
> 由外边距边界限制，用空白区域扩展边框区域，以分开相邻的元素。它的尺寸为 margin-box 宽度 和 margin-box 高度。

外边距区域的大小由 margin-top、margin-right、margin-bottom、margin-left，和简写属性 margin 控制。在发生外边距合并的情况下，由于盒之间共享外边距，外边距不容易弄清楚。

> 最后，请注意，除可替换元素外，对于行内元素来说，尽管内容周围存在内边距与边框，但其占用空间（每一行文字的高度）则由 line-height 属性决定，即使边框和内边距仍会显示在内容周围。