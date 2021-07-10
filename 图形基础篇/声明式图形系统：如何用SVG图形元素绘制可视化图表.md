## 声明式图形系统：如何用SVG图形元素绘制可视化图表?

> SVG 的全称是 Scalable Vector Graphics，可缩放矢量图，它是浏览器支持的一种基于 XML 语法的图像格式

SVG 坐标系和 Canvas 坐标系完全一样，都是以图像左上角为原点，x 轴向右，y 轴向下的左手坐标系。而且在默认情况下，SVG 坐标与浏览器像素对应，所以 100、50、40 的单位就是 px，也就是像素，不需要特别设置。

## SVG和canvas的不同点

1. 写法不同，SVG通过创建标签来表示图形元素，circle表示圆形，g表示分组，text表示文字，通过元素的setAttribute给图形元素赋属性值，和操作HTML是一样的；canvas是通过上下文执行绘图指令来绘制图形，圆是调用context.arc指令，然后调用context.fill绘制，画文字则是调用context.fillText指令，另外，设置状态属性，也是通过上下文，如context.fillStyle设置填充颜色。
2. 用户交互实现上的不同，SVG有个非常大的优点，就是**可以让图形的用户交互非常简单**，和SVG相比，利用canvas对图形元素进行用户交互就没那么容易了。

解决绘制大量集合图形时SVG的性能问题：虚拟DOM，尽可能减少重绘，如果节点数太多，还是得依靠canvas和WebGL来绘图才能彻底解决问题。

思考：如何确定鼠标是否在某个图形元素内部？

相关内容：[SVG精髓](https://github.com/wangmeijian/svg)