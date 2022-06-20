---
title: Tesla AI Day
tags: 文字
key: article-tesla-ai-day
pageview: true
toc: false      
---

<!--
 * @Date: 2020-04-21 08:06:52
 * @LastEditTime: 2022-06-20 22:40:58
 * @LastEditors: Li Xiang
 * @Description: 
 * @FilePath: \notlixiang.github.io\_posts\2022-06-20-tesla-ai-day.md
-->

<style type="text/css">
	mark { 
        background-color:grey; 
        color:grey; 
    } 
</style>

[[Youtube Link](https://www.youtube.com/watch?v=j0z4FweCy4M&ab_channel=Tesla)]

鸽了好久，先从简单的内容开始写吧。

2021年8月19日的AI Day上，Tesla在其中介绍了其基于纯视觉的感知系统。

其模型部分主要具有以下特点：多任务共享主干(RegNet+BiFPN)，利用transformer互注意力将主干提取的多相机图像特征转换到BEV表示中(Vector Space)，并在时间维度进行融合。

![](https://raw.githubusercontent.com/notlixiang/notlixiang.github.io/master/_posts/images/2022-06-20-22-04-00.png)

另一张示意图对Tesla感知模型的transformer构造原理进行了更详细的解析，附上medium链接供参考原文。

![](https://raw.githubusercontent.com/notlixiang/notlixiang.github.io/master/_posts/images/2022-06-20-22-04-53.png)
[source](https://medium.com/towards-data-science/monocular-bev-perception-with-transformers-in-autonomous-driving-c41e4a893944a)

据笔者所知，Tesla是第一家公开利用transformer实现自动驾驶bev感知方案的厂商(当然别的厂商也没公开过)，后续在学界和业界都引起了一阵transformer bev感知的风潮(nuscenes榜单上vision track的top都是bev方案，大部分都与transformer相关)。

当然，Tesla的方案还是有一些待改进点的。Vanilla transformer做cross view transform这个任务最大的问题是相机位置与模型参数绑定，即换个车型，相机配置(数量，内外参)发生变化，整个网络，甚至数据集，都得重新构造；当然，Tesla肯定不止这一套方案，而且talk中也提到利用虚拟镜头进行图像映射，以及通过仿真构造数据集，这些都是解决这个问题的办法(而且人家每个车型量产这么多，稍微搞点数据对每个车型的网络分别微调也完全可行)。当然，除此之外，网络进行修改和适当的数据增强以后，也可以实现一个网络适配任意车型。

第二点改进点来自于transformer本身的特性，推理算力需求高，算子零散化，训练收敛慢，数据量要求高，这些都对网络设计及部署，甚至数据工程提出了新的需求(果然Tesla敢放出来，这就不是一般公司小打小闹就能玩得起的，软硬件都要行，还得砸钱搞数据)。

说实话，现在Tesla已经成为智能汽车领域的苹果了，新的HMOV何在(狗头)。
