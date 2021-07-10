## 指令式绘图系统：如何用Canvas绘制层次关系图？

### 画布宽高
> Canvas 的 HTML 属性宽高为画布宽高，CSS 样式宽高为样式宽高

所谓画布宽高，就是在操作canvas的时候，使用的坐标，如：

在一个画布宽高为500\*500、样式宽高为200\*200的canvas上，从坐标（300，300）处画一条线到坐标（400, 400），这里的坐标（300，300）和（400，400）就是基于画布宽高，而非基于样式宽高。

不设置canvas的HTML属性宽高时，canvas的画布宽高 = 样式宽高

重要：每次使用canvas，都设置画布宽高，这样做的好处是，不管canvas样式宽高如何改变，我们绘制的图形都不需要再重新计算坐标。

假设我们想绘制一个中心点在画布中心的正方形，有两种方式：

1. 我们可以让 rect 指令的 x、y 参数，等于画布宽高的一半分别减去矩形自身宽高的一半
2. 我们可以先给画布设置一个平移变换（Translate），然后再进行绘制

两种方式有什么区别呢？

第1种，简单！它直接改变了要绘制的图形顶点坐标位置，如果我们绘制的是很多顶点的图形，就需要在绘制前计算出每个顶点的位置，这就非常麻烦

第2种，是对canvas画布的整体做一个平移操作，我们只需要获取中心点与左上角的偏移，然后对画布设置translate变换就可以了。不过，这样一来我们就改变了画布在状态，如果后续还有其他图形要绘制，一定记得把画布状态给恢复回来。好在，这也不会影响到已经画好的图形。

恢复画布状态也有两种方式：

1. 反向平移
2. canvas上下文提供了save和restore方法，可以暂存和恢复某个时刻的绘制状态，执行save()，类似于生成了一个快照，在执行restore()的时候，将绘制状态恢复到快照的状态。绘制状态包括（来源：[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/save)）：

- 当前的变换矩阵
- 当前的剪切区域
- 当前的虚线列表
- 以下熟悉的当前值：strokeStyle, fillStyle, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit, lineDashOffset, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, globalCompositeOperation, font, textAlign, textBaseline, direction, imageSmoothingEnabled.

## canvas的优缺点
### 优点
canvas是一个非常简单易用的图形系统，并且canvas渲染起来相当高效。

### 缺点
很难直接抽取其中的图形对象进行操作，相较于HTML或者SVG，我们可以一一获取这些图形然后给他们绑定事件。



