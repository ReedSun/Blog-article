# 关于Flex，有12个属性很重要
这几天在学习`Flex`布局，发现`Flex`真的好厉害！

Flex是Flexible Box的缩写，意为“弹性布局”，用来为盒模型提供最大的灵活性。

`Flex`是它可以简单、完整、响应式的实现各种网页布局，目前已经得到了大多数主流浏览器的支持，有关于它的兼容性可以在`CanIuse`中的查询到：

![](http://i1.piimg.com/567571/28c7e024eb4e68b0.png)

任何一个容器都可以指定为Flex布局

```
.box {
    display: flex;
}
.box {
    display: inline-flex;  // 行内元素通过这种方式指定为Flex布局
}
.box {
    display: -webkit-flex  // Webkit内核浏览器使用此方式指定为Flex布局
}
```

这篇文章是我对`Flex`的总结，文章中的内容主要借鉴自[Flex布局教程 by阮一峰](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)、[flex布局 by饥人谷方方老师](http://www.jirengu.com/app/video/1412)。

## 基本概念
- `flex container`：采用`Flex`布局的元素，即父元素，称为**Flex容器**，简称容器。
- `flex item`：父元素内包含的子元素，称为**Flex项目**，简称项目。
- Flex是没有方向之分的，在Flex容器中默认存在两根轴，水平的轴为 **主轴main axis**，垂直的轴为 **侧轴cross axis**。（如果改变`flex-direction`，主轴和侧轴也将会改变）
- 主轴的开始位置（与边框的交叉点）叫做 **main start** ，结束位置叫做 **main end** 。
- 侧轴的开始位置叫做 **cross start** ， 结束位置叫做 **cross end** 。
- 项目默认沿主轴方向排列，单个项目占据的主轴空间叫做 **main size** ，侧轴空间叫做 **cross size** 。

以上概念可以用下图全部展现：

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png)

注意：设为Flex布局后，子元素的`float`、`clear`、`vertical-align`属性将失效。

## 容器（父元素）的属性
容器共有六个属性

- `flex-direction`
- `flex-wrap`
- `flex-flow`
- `justify-content`
- `align-items`
- `align-content`

### flex-direction属性
`flex-direction`属性决定主轴的方向，它可能有四个值。

- `row`：默认值，主轴为水平方向，起点在左端。
- `row-reverse`：主轴为水平方向，起点在右端。
- `column`：主轴为垂直方向，起点在上端。
- `column-reverse`：主轴为垂直方向，起点在下端。

四种值对应的情况如下图所示：

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071005.png)

### flex-wrap属性
`flex-wrap`属性决定项目在一行排不下的情况下是否换行，它可能有三种值。

- `nowrap`：默认值，不换行。
- `wrap`：换行，第一行在主轴开始方向，依次往主轴结束方向布置。
- `wrap-reverse`：换行，第一行在主轴结束方向，依次往主轴结束方向布置

三种值对应的情况如下图所示：

![](http://i1.piimg.com/567571/53b72f333114133a.png)

### flex-flow属性
`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。
### justify-content属性
`justify-content`属性定义了项目在主轴上的对齐方式，它可能有五个值。

- `flex-start`：默认值，主轴开始方向对齐。
- `flex-end`：主轴结束方向对齐。
- `center`：主轴居中对齐。
- `space-between`：两端对齐，项目之间间隔都相等。
- `space-around`：每个项目两侧的间隔相等，所以项目之间间隔是项目与边框间隔的两倍。
假设主轴为从左到右，五种值对应的情况如下图所示：

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png)

### align-items属性
`align-items`属性定义了每行项目在侧轴方向上的对齐方式，它也可能有五个值。
- `flex-start`：侧轴开始方向对齐。
- `flex-end`：侧轴结束方向对齐。
- `center`：侧轴居中对齐。
- `baseline`：项目第一行文字的基线对齐
- `stretch`：默认值，如果项目未设置高度或高度设为auto，将占满整个容器。
假设侧轴为从上到下，五种值对应的情况如下图所示：

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png)

### align-content属性

`align-content`属性定义了容器在侧轴方向上有额外空间时，如何每排布一行，当容器只有一行时，它不起作用，它可能有六个值。

- `flex-start`：侧轴开始方向对齐。
- `flex-end`：侧轴结束方向对齐。
- `center`：侧轴中心中对齐。
- `space-between`：与侧轴两端对齐，每行轴线间隔平均。
- `space-around`：每根轴线两侧间隔相等。
- `stretch`：默认值，占满整个整个侧轴

假设侧轴为从上到下，六种值对应的情况如下图所示：

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071012.png)

## 项目（子元素的属性）
项目共有六个属性

- `order`
- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex`
- `align-self`

### order属性
`order`属性定义项目的排列顺序，数值越小排列越靠前，默认为0，可能的值为任意整数。

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071013.png)

### flex-grow属性
`flex-grow`属性定义项目的放大比例，默认为0，即如果存在剩余空间也不放大。

如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍，如下图所示：

![](http://p1.bqimg.com/567571/5046fad398a2c988.png)

### flex-shrink属性
`flex-shrink`属性定义了项目的缩小比例，默认为1，即如果空间不足该项目将缩小。

负值对该属性无效,即该属性可能的值为0或正整数。

如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小，如下图所示：

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071015.jpg)

### flex-basis属性
`flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（main-size）。浏览器根据整个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。

### flex属性
`flex`属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选。

该属性有两个快捷值：`auto (1 1 auto)`（既可以放大占满空间，也可缩小） 和 `none (0 0 auto)`（不可放大，也不可缩小）。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

### align-self属性
`align-self`属性允许单个项目有与其他项目不一样的侧轴对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

其值除`auto`外，其他与`align-items`完全一致。

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071016.png)

## 总结
只要记住了这12个属性，使用Flex布局就肯定没问题啦，在我的GitHub上有几个关于Flex布局的小例子，想复习的朋友可以去试一试，[仓库地址在这里](https://github.com/ReedSun/flex-demo)。

(完)