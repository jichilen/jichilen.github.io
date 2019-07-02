```
layout: post
title:  "Bottom-up Object Detection by Grouping Extreme and Center Points"
date:   2019-6-12
desc: "Bottom-up Object Detection by Grouping Extreme and Center Points"
keywords: "python"
categories: [Html]
tags: [reading,object,points]
icon: icon-html
```

这篇文章与传统的物体检测不同

不在检测anchor而是去检测四个极值点（左右上下）以及中心点

> 显然只预测四个点是对于分组来说困难的，我们无法判断一个图中某类物体出现的次数，因此需要有一个辅助判断的东西，这篇论文使用的是中心点，这也是比较直观的一种方式

这样可以把物体检测转换成一个纯外观的关键点估计问题，不需要分类或者是隐式特征的学习

矩形边框不是自然对象表示。

> （这一点说的很对所以我选择做分割）

自上而下的方式统治了目标检测很久

> （分割能否认为是一种自下而上的方法？）

我们将四个极值点分组。分组的依据是当且仅当它们的几何中心在中心热图中以高于预定义阈值的值进行预测时。这样的话运算复杂度是$O(n^4)​$。

> 这种模式没有问题吗，如果错误的四个点计算的中心正好也大于阈值该怎么办？

具体的流程如图：

![1](..\assets\img\paper-6-1.jpg)

> 对于这个图我就有问题了，这个长颈鹿的center heatmap明显就不合常理，上面说可以抛弃隐式特征的学习一级分类，我觉得并没有做到

**与Corner Net的区别**：1.Corner是另一种形式的bbox，经常出现在区域外面。而极值点出现在物体边界，在视觉上容易区分。2.分组的方法，Corner Net 用的是一个关联特征，而这里完全是基于外观的。

> 对于第一点我是认同的，第二点我认为是作者的无奈之举，并没有出彩的地方。

**关键点检测的一些做法**：使用全卷积的encoder-decoder模型也预测多通道的热度图。训练的时候使用L2loss，这个时候gt是高斯图，或者使用逐像素的逻辑回归的loss。

**网络基础结构**：主要还是Corner Net的结构，HourglassNet作为基础网络，预测4类极值点以及中心点，这个预测是分类别的，然后预测一个offset，这个是无类别的，这样就是一个$5\times C +4\times 2$的热度图。gt使用的是多峰值的高斯函数，高斯的方差和物体在图中的尺度相关。

测试的时候提取$3\times3$峰值作为预测的关键点，得到四类关键点使用一个非常简单的算法（它们的几何中心在中心热图中的值以高于预定义阈值），当然他们也要满足定义上的约束，每个极值点都在他们确定的位置

> 可以明显的看到，由于是求$3\times 3$的峰值，肯定会有很多区域呈现高响应，可以预见的是一个nms是必须的

**ghost box 抑制**，这里作者解决了之前考虑的问题，如果错误的匹配结果的几何中心正好落在中心点的高响应区域，那么就认为这是一个ghost box，显然，ghost box一般会包含多个small box，作者这里使用了一个soft nms来解决这个问题。   如果某一边界框中包含的所有框的得分之和超过3乘以它自身的分数，除以它的分数2。

> 其实这里和文字检测有很大的关系，如果我们能够分类中英文，对于中文我们就可以考虑使用这种类似的抑制方式

**水平或者垂直边缘**:作者这里使用了边缘聚合的方式，也就是说他认为一个水平或者垂直边缘的模式应该是一个高原响应$(/^{——————}\backslash)$的形式 ，具体的计算公式为$\widetilde Y_m=\hat Y_m +\lambda_{aggr}\sum^{i_1}_{i=i_0}N_i^{(m)}$

> 个人觉得这里有明显的问题，不能保证水平边缘单调的区间就更大，这样只能提高所有的峰值的响应值



> 综合上面的考虑，使用这着极值点来做检测有着下面几个明显的问题
>
> - [x] 水平或者垂直边缘
> - [x] 极值点的分组（聚合）
> - [x] 错误的匹配结果的几何中心正好落在中心点的高响应区域
> - [ ] 漏检
> - [x] 后处理算法复杂度过高
> - [x] 标注