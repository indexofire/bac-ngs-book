## circos

circos 是特别著名的以圈图展示关系型数据的可视化应用程序。它并非只是用于展现基因组数据，而是可以解析各种相关数据的应用程序。

在它的圈图体系中，各种 tracks 可以分成一下一种表现图示：

* Histogram 直方图
* Ideogram 染色体示意图
* inverted Histogram 反向直放图
* Heatmap 热力图
* Links 关系线
* Highlights 加亮图示
* Ticks 圈图标尺
* Grid 圈图网格

绘制一个circos图时，首先要考虑的是如何理解并展示复杂的数据关系。通过不同维度来解析数据，

```
$ circos -conf circos.conf
```
