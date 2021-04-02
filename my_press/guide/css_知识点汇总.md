---
title: css知识点汇总
category: CSS
tag:
  - css
comment: false
---
# display的值有哪些
- block：块类型，元素独占一行
- none：元素不显示
- inline：行内类型，不可设置宽高
- inline-block：行内块类型，拥有行内样式的特点，但是不可以设置宽高
- flex：弹性盒模型
- table：作为块级表格显示
- inherit：从父元素继承display值

# position 定位
- absolute：绝对定位，相对于值不为static的第一个父元素的padding box进行定位，也可以理解为离自己这一级元素最近的
一级position设置为absolute或者relative的父元素的padding box的左上角为原点的。
- relative：相对定位，相对于元素本身
- fixed：固定定位，相对于浏览器窗口
- static：默认值。没有定位，元素出现在正常的流中（忽略top,bottom,left,right,z-index声明）。

# flex弹性盒模型
Flex是FlexibleBox的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

任何一个容器都可以指定为Flex布局。行内元素也可以使用Flex布局。注意，设为Flex布局以后，子元素的float、cl
ear和vertical-align属性将失效。

采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex
项目（flex item），简称"项目"。

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis），项目默认沿主轴排列。


以下6个属性设置在容器上。

flex-direction属性决定主轴的方向（即项目的排列方向）。

flex-wrap属性定义，如果一条轴线排不下，如何换行。

flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

justify-content属性定义了项目在主轴上的对齐方式。

align-items属性定义项目在交叉轴上如何对齐。

align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。


以下6个属性设置在项目上。

order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。它的默认
值为auto，即项目的本来大小。

flex属性是flex-grow，flex-shrink和flex-basis的简写，默认值为0 1 auto。

align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父
元素的align-items属性，如果没有父元素，则等同于stretch。

# 纯css实现三角形
```javascript
//  采用的是相邻边框连接处的均分原理。
//  将元素的宽高设为0，只设置
//  border
//  ，把任意三条边隐藏掉（颜色设为
//  transparent），剩下的就是一个三角形。
  #demo {
  width: 0;
  height: 0;
  border-width: 20px;
  border-style: solid;
  border-color: transparent transparent red transparent;
}
```

# chrome 会将小于12px的字体强制转换为12px显示的解决方案
1. 可通过加入CSS属性-webkit-text-size-adjust:none;解决。但是，在chrome
更新到27版本之后就不可以用了。
2. 还可以使用-webkit-transform:scale(0.5);注意-webkit-transform:scale(0.75);
收缩的是整个span的大小，这时候，必须要将span转换成块元素，可以使用display：block/inline-block/...；

# 怪异模式问题
漏写DTD声明，Firefox仍然会按照标准模式来解析网页，但在IE中会触发怪异模式。为避免怪异模式给我们带来不必要的麻烦，最好养成书写DTD声明的好习惯。

# li与li之间的空白问题
当我们给li设置display:inline-block为了让li都在一行显示时，最后发现实际效果元素间可能会存在空白，这就是因为inline-block会将换行渲染为空格，因此我们可以通过
- 将inline-block元素写在同一行解决；
- 可以通过设置父元素字体大小为0，但是元素中其他字体也会受影响，需要单独设置字体；
- 可以通过letter-spacing设置字符间距，但是也会影响子元素，需要为子元素单独设置；

# width:100%和width:auto的区别
- width:100%表示，当前元素的高度填满元素的content-box区域;
- width:auto表示，子元素的 content+padding+border+margin 等撑满父元素的 content 区域。

# 绝对定位元素与非绝对定位元素的百分比计算的区别
- 绝对定位元素的宽高百分比是相对于临近的position不为static的祖先元素的padding box来计算的。
- 非绝对定位元素的宽高百分比则是相对于父元素的content box来计算的。

# clear属性
**概念：** 属性指定一个元素是否必须移动(清除浮动后)到在它之前的浮动元素下面。

**作用元素：** 块级元素

## clear:both 清除浮动原理
```
    .father::after{
        content:'';
        clear: both;
        display: block;
        height: 0;
    }
```
父元素的伪元素设置clear:both清除左右浮动并占位，伪元素始终在浮动元素的下面，所以撑起了父元素的高度。

# css样式重置
## 通配符方式
- 优点：写法简单
- 缺点：
    - 需要把所有的标签都遍历一遍，当网站较大时，
样式比较多，这样写就大大的加强了网站运行的负载，会使网站加载的时候需要很长一段时间，因此一般大型的网站都有分层次的一
套初始化样式。
    - 而且重置样式并不是绝对的将margin、padding设置为0 

# 包含块
一个元素的尺寸和位置经常受其包含块(containing block)的影响。大多数情况下，包含块就是这个元素最近的祖先块元素的内容区，但也不是总是这样。

一个盒子元素包括四个区域，从内到外依次为content、padding、border、margin

包含块的查找规则：
1. 如果 position 属性为 static 、 relative 或 sticky，包含块可能由它的最近的祖先块元素（比如说inline-block, block 或 list-item元素）的内容区的边缘组成，也可能会建立格式化上下文(比如说 a table container, flex container, grid container, 或者是 the block container 自身)。
2. 如果 position 属性为 absolute ，包含块就是由它的最近的 position 的值不是 static （也就是值为fixed, absolute, relative 或 sticky）的祖先元素的内边距区的边缘组成。
3. 如果 position 属性是 fixed，在连续媒体的情况下(continuous media)包含块是 viewport ,在分页媒体(paged media)下的情况下包含块是分页区域(page area)。
4. 如果 position 属性是 absolute 或 fixed，包含块也可能是由满足以下条件的最近父级元素的内边距区的边缘组成的：
    1. transform 或 perspective 的值不是 none
    2. will-change 的值是 transform 或 perspective
    3. filter 的值不是 none 或 will-change 的值是 filter(只在 Firefox 下生效).
    4. contain 的值是 paint (例如: contain: paint;)

width、height、padding、margin的百分比都是根据包含块来计算的

## 扩展问题：为什么height:100%会生效
我们可以使用包含块来回答这个问题，示例代码：
```
<html>
    <body>
      <div></div>  
    </body>
</html>
```
我们想让div高度为整个窗口的高度，当我们给div设置height:100%的时候，通常不会生效，由第一条规则可知，包含块由内到外为body-html，所以只有当同时给body和html设置高度100%的时候才可以达成我们想要的效果。

# chrome支持小于12px文字设置
1. 设置-webkit-text-size-adjust：none，但是chrome27之后无法使用
2. 使用scale缩放字体\

# 请求静态资源的时候不携带cookie的方案
静态资源提交到外域CDN上解决

# inline-block相邻元素出现的元素间距
**产生原因：** 因为代码在格式化的时候会出现回车，回车在编译过程中被编译成了空格所以相邻元素出现了空白
**解决方案：**
1. margin负值
2. font-size:0
3. 相邻元素在代码层面上不进行格式化，写在同一行

# transition和animation的区别
transition关注的是CSS property的变化，property值和时间的关系是一个三次贝塞尔曲线。

animation作用于元素本身而不是样式属性，可以使用关键帧的概念，应该说可以实现更自由的动画效果。

# margin：auto填充规则
1. 一侧为定值，另一侧设置auto时会自动分配空间
2. 两侧auto则评分剩余空间

# margin无效场景
1. 元素宽度定宽无法设置margin-right
2. 元素绝对定位并确定定位方向top、right、bottom、left时，则设置它的同向margin无效。 如我设置right:10px向左定位时，则设置margin-left是无效的。