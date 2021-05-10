---
title: 多列等高布局的实现
category: CSS
tag:
  - css
  - 多列等高布局
  - 负margin
comment: false
---

# 负 margin，正 padding 对冲

## 场景演示

```
<style>
    .container {
        padding: 10px;
        float: left;
    }
    .article {
        float: left;
        width: 300px;
        margin-right:10px;
        background: rgb(246, 225, 184);
    }
</style>
<body>
   <div class="container">
      <p class="article">
        北国风光，千里冰封，万里雪飘。
        望长城内外，惟余莽莽；大河上下，顿失滔滔。
        山舞银蛇，原驰蜡象，欲与天公试比高。 须晴日，看红装素裹，分外妖娆。
        江山如此多娇，引无数英雄竞折腰。
        惜秦皇汉武，略输文采；唐宗宋祖，稍逊风骚。
      </p>
      <p class="article">
        庆历四年春，滕子京谪守巴陵郡。越明年，政通人和，百废具兴，乃重修岳阳楼，增其旧制，刻唐贤今人诗赋于其上，属予作文以记之。
        予观夫巴陵胜状，在洞庭一湖。衔远山，吞长江，浩浩汤汤，横无际涯，朝晖夕阴，气象万千，此则岳阳楼之大观也，前人之述备矣。然则北通巫峡，南极潇湘，迁客骚人，多会于此，览物之情，得无异乎？
        若夫淫雨霏霏，连月不开，阴风怒号，浊浪排空，日星隐曜，山岳潜形，商旅不行，樯倾楫摧，薄暮冥冥，虎啸猿啼。登斯楼也，则有去国怀乡，忧谗畏讥，满目萧然，感极而悲者矣。
        至若春和景明，波澜不惊，上下天光，一碧万顷，沙鸥翔集，锦鳞游泳，岸芷汀兰，郁郁青青。而或长烟一空，皓月千里，浮光跃金，静影沉璧，渔歌互答，此乐何极！登斯楼也，则有心旷神怡，宠辱偕忘，把酒临风，其喜洋洋者矣。
        嗟夫！予尝求古仁人之心，或异二者之为，何哉？不以物喜，不以己悲，居庙堂之高则忧其民，处江湖之远则忧其君。是进亦忧，退亦忧。然则何时而乐耶？其必曰“先天下之忧而忧，后天下之乐而乐”乎！噫！微斯人，吾谁与归？
        时六年九月十五日。
      </p>
      <p class="article">
        环滁皆山也。其西南诸峰，林壑尤美，望之蔚然而深秀者，琅琊也。山行六七里，渐闻水声潺潺，而泻出于两峰之间者，酿泉也。峰回路转，有亭翼然临于泉上者，醉翁亭也。作亭者谁？山之僧智仙也。名之者谁？太守自谓也。太守与客来饮于此，饮少辄醉，而年又最高，故自号曰醉翁也。醉翁之意不在酒，在乎山水之间也。山水之乐，得之心而寓之酒也。
        若夫日出而林霏开，云归而岩穴暝，晦明变化者，山间之朝暮也。野芳发而幽香，佳木秀而繁阴，风霜高洁，水落而石出者，山间之四时也。朝而往，暮而归，四时之景不同，而乐亦无穷也。
        至于负者歌于途，行者休于树，前者呼，后者应，伛偻提携，往来而不绝者，滁人游也。临溪而渔，溪深而鱼肥。酿泉为酒，泉香而酒洌；山肴野蔌，杂然而前陈者，太守宴也。宴酣之乐，非丝非竹，射者中，弈者胜，觥筹交错，起坐而喧哗者，众宾欢也。苍颜白发，颓然乎其间者，太守醉也。
        已而夕阳在山，人影散乱，太守归而宾客从也。树林阴翳，鸣声上下，游人去而禽鸟乐也。然而禽鸟知山林之乐，而不知人之乐；人知从太守游而乐，而不知太守之乐其乐也。醉能同其乐，醒能述以文者，太守也。太守谓谁？庐陵欧阳修也。
      </p>
    </div>
</body>
```

最初效果如下图所示：
![](http://image.followmyheart.cn/1617114466%281%29.jpg)
但是我们需要的效果是让这三列高度相同，目前我知道的实现方案主要有四种：

- 弹性盒模型默认会撑起元素高度
- 固定子元素高度
- 负 margin 与正 padding 对冲
- js 动态计算高度

第一种可兼容所有主流浏览器和 ie9+；第二种是最不推荐的，用户体验极差；第三种兼容性是最好的，可以兼容早期 ie；第四种的性能可以说是最差了

在这四种的选择上，目前在开发的时候大部分情况不需要考虑低版本 ie，所以我首选弹性布局。

## 代码实现

```
<style>
    .container {
        padding: 10px;
        float: left;
        overflow:hidden; // 新增
    }
    .article {
        float: left;
        width: 300px;
        margin-right:10px;
        margin-bottom: -9999px; // 新增
        padding-bottom: 9999px; // 新增
        background: rgb(246, 225, 184);
    }
</style>
```

最终效果：
![](http://image.followmyheart.cn/1617114610%281%29.png)

## 知识点介绍

这里我们主要介绍负 margin 在使用上的一些技巧

> 注：负 margin 并不是 hack，他是符合 css 规范的

### 代码

```
    <style>
      .el {
        width: 150px;
        height: 150px;
      }
      .el-1 {
        opacity: 0.5;
        background-color: orange;
      }
      .el-2 {
        background-color: pink;
      }
    </style>
    <body>
        <div class="el el-1"></div>
        <div class="el el-2"></div>
    </body>
```

![](http://image.followmyheart.cn/init.png)

我们通过对 el-1 设置不同的 margin 来对比效果

#### margin-left

![](http://image.followmyheart.cn/ml10.png)

#### margin-top

![](http://image.followmyheart.cn/mt10.png)

#### margin-bottom

![](http://image.followmyheart.cn/mb10.png)

#### margin-right

![](http://image.followmyheart.cn/mr10.png)

#### 总结

通过常看以上效果图，我们不难发现：

- margin-left 和 margin-top 的作用效果是类似的，都是释放空间，同时目标元素产生位移，其他元素的位置也会收到影响；
- margin-right 和 margin-bottom 类似，他们是目标元素不会产生位移，只是会释放空间，同时也会影响子元素的位置。
  > 其中的原理和[包含块](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Containing_block)有关

## 实现原理

将我们的 margin-bottom 设置足够大（9999），同时我们也需要设置足够大（9999）的 padding-bottom 用来占用其余空间，当我们内容最多的元素撑起父元素高度时，其他元素的高度也就和内容最多的元素等高了。
