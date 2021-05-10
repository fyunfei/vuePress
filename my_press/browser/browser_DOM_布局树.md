---
title: DOM树和布局树
category: 浏览器
tag:
  - 浏览器
  - chrome
  - DOM树的构建
  - 布局树构建
comment: false
---
# DOM树
## 什么是DOM
- 从页面来看，DOM是创建页面的基础数据结构
- 从Javascript来看，DOM提供Javascript可以操作页面元素的接口
- 从安全来看，DOM 是一道安全防护线，一些不安全的内容在 DOM 解析阶段就被拒之门外了。

## DOM树的生成
当我们在浏览器情求html页面时，html会返回对应的字节流，此时还不是DOM结构，需要通过**HTML解析器**将字节流转换成DOM。
注：解析过程是随着网络资源边加载边解析的

### DOM树生成过程
- **分词器**将字节流转换为Token，例如 StartTag、EndTag、Text
- 用一个栈结构对Token进行处理，当为StartTag时入栈，并创建对应DOM节点；如果是文本则直接添加到DOM树中；如果为EndTag时，则将对应的StartTag出栈。

举个例子：
```
<html>
  <body>
    <h1>标题测试</h1>
    <p>内容测试</p>
  </body>
</html>
```
此时对应的Token栈应该为

```
    [StartTag html, startTag body, startTag h1] 
    此时会遇到endTag h1则startTag h1出栈，startTag p入栈结构变成
    [StartTag html, startTag body, startTag p] 
```

对应树形结构

![](http://image.followmyheart.cn/1620561032%281%29.png)

## DOM的生成都收到那些因素的影响？
这里主要受到两个因素的印象javascript、和网络因素

### javascript影响DOM的生成
在进行DOM树的构建过程中，在遇到script标签时，会暂停DOM树的构建，直到script中的脚本执行完毕

### 网络因素影响DOM的生成
当我们用script标签加载外部资源脚本的时候，此时会在网络进程发起情求，因此就增加了响应的时间。

### 如何解决？
- Chrome在接收到字节流后会进行一次预解析，会提前加载脚本。
- 在保证网络速度稳定的情况下，可以减小脚本文件的大小（通过压缩，模块化拆分，懒加载）进而减少js层面在DOM生成的影响
- 可以使用内联脚本减少网络进程的情求过程
- 使用CDN托管执行脚本
- 与DOM操作无关的脚本可以使用async属性

# 布局树

## 什么是CSSOM
与DOM类似，渲染引擎无法识别CSS，所以需要将CSS转换为渲染引擎可以识别的结构CSSOM；CSSOM具有两个作用：一个是提供Javascript操作样式表的能力；第二个是为布局树的合成提供基础的样式信息。

当我们DOM和CSSOM都准备好以后，渲染进程就会进行布局树的创建。

## 布局树的创建
布局树和DOM树的差别在于，布局树会将display：none的元素过滤，这其中的过程主要分为两部**样式计算**、**布局计算**。

**样式计算**主要是对DOM选择对应的样式信息。

**布局计算**是计算布局中每个元素对应的几何位置。

这两步结束之后就会进行对应的绘制操作了。

## CSS会影响DOM的生成吗
答案不是绝对的，因为 JavaScript 有修改 CSSOM 的能力，所以在执行 JavaScript 之前，还需要依赖 CSSOM。也就是说 CSS 在部分情况下也会阻塞 DOM 的生成。

接下来我们分析一下以下的渲染流程：
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="/style.css" />
  </head>
  <body>
    <div>测试</div>
    <script>
      let s = document.querySelector("div");
      s.style.color = "#f00";
    </script>
  </body>
</html>
```
使用Chrome性能分析面板简单分析以下流程（Performance）
![](http://image.followmyheart.cn/performance.png)
- Chrome预解析加载并解析生成CSSOM
- 遇到script标签，暂停当前DOM生成，执行脚本，因为此时执行样式相关的脚本会触发样式重新计算
- 最后进行布局计算