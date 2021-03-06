---
layout: post
title:  "为什么VR电影没有沉浸感"
date:   2016-12-13 11:01:30 +0800
categories: vive tech
---

本文将介绍什么是视差、为什么不完善的视差是导致现在的VR电影沉浸感不强的原因、可能的解决方案。

目前的VR电影有以下缺陷：


- 	焦距固定
- 	双眼视差仅在水平方向正常工作
- 	焦距和双眼视差不匹配导致体验不舒适
- 	没有移动视差


这些问题严重影响了现在的VR电影体验，本文将说明为什么。

> 注意，这里说的VR电影指的是用摄像机拍摄的立体全景电影，不包括实时渲染的电影。


## 什么是视差

**视差(parallax)** 是指在观察同一物体时，因为观察点不同导致观察到不同结果的现象。例如：

![screenshot1](/assets/why_arent_vr_movies_immersive/1.png)

图中观察对象是星星，背景是三个不同颜色的方块。如果观察点在A点，就会发现星星在蓝色背景之上；而如果观察点在B点，结果就是星星在红色背景之上。

视差的例子有很多，天文学家用视差来测算星星到地球的距离。 许多动物的两只眼睛有着重叠的视野，可以利用视差获得**深度知觉(depth perception)** 。一个简单的，日常都能见到的视差例子是，汽车仪表板上"指针"显示的速度计。当从正前方观看时，显示的正确数值可能是60；但从乘客的位置观看，由于视角的不同，指针显示的速度可能会略有不同。

从计算机视觉的角度来解释，视差就是从不同位置拍摄的图像以及能从图像中获得的位置信息。以下图为例，同一个场景在两个摄像机的拍摄下，生成了两张不同的图像。很明显在两个图像中，两个被观察的物体的相对位置和绝对位置都不一样。

![screenshot2](/assets/why_arent_vr_movies_immersive/2.png)

## 能提供距离感的视差

### 距离线索的分类（传统）

在说明视差和VR的联系之前，需要提一下**沉浸感(immersion)** 。沉浸感就是感觉自己不是在看2D的图像，而是身在3D场景中。而要感觉自己在3D场景中，其中最重要的一个条件就是**距离感(depth)** 。你能感觉出来哪些物体离你比较近，哪些离你比较远。人是非常擅长判断距离的，我们能依靠多个**线索(cue)** ，下面列举比较有代表性的几种：


```
单眼：
     几何：大小，阻挡，透视，运动视差
     颜色：光照，纹理梯度，空气透视（近处物体更清晰）
     聚焦：调焦，视网膜模糊
双眼：辐辏，视网膜差异
```

这是比较常见的一种分类方式，把提供距离感的线索分成**单眼(monocular**) 和**双眼(binocular)** 。从这个表可以得出什么结论呢？

> 我们平时总能听到一个说法，只有双眼视差才能提供距离感。这么说显然是不对的。从理论上说，上面这个表提到的距离线索里只有两条是与双眼有关的。从实践经验来说，我们闭上一只眼其实也能感知到距离；在玩RPG游戏时，我们通过普通的显示器，也能把画面脑补成3D的。


### 距离线索分类（根据视差）

当然，想要获得沉浸感，有几个线索是必不可少的。那哪些线索在VR电影中有容易出问题，需要额外注意？我们先把它们重新分类一下：


```
非视差线索：
     几何：大小，阻挡，透视
     颜色：光照，纹理梯度，空气透视（近处物体更清晰）
视差线索：
     单眼：头部运动，运动视差
     双眼：辐辏，视网膜差异
     聚焦：调焦，视网膜模糊
```

     
这么分类有两个原因，一是很明显这些线索里有一半都是依靠视差的，二是非视差的线索在现在的VR电影中实现得不错，本文就不再做讨论。本文主要分析这些视差相关的线索在VR电影中实现得如何。

请注意这些视差线索是**成对出现**的，单眼、双眼和聚焦都分别有一个**物理线索(physical)**（左）和一个**图像线索(image-based)**（右）。
(接下来视差线索简称视差。)

### 运动视差(motion parallax)：

要体验运动视差只需要左右摆头部，这么做时你会发现视野中的物体都在运动，近处的物体移动比较多，远处的物体移动较少。这个体验就是头部运动视差线索。它也分两部分：

- 物体线索：大脑能感知到头移动了多少（即使闭上眼睛也能感知到）
- 图像线索：视野里的物体绝对位置和它们之间的相对位置都发生了变化
     
这两个线索结合起来成为了一个单眼视差线索。如果一个HMD实现了这个，只要让用户稍微移动一下头，就能知道很多其他方式所无法得到的距离信息。


### 双眼视差(binocular parallax)：

把一跟手指放到眼前，然后拉近距离，只要你一直看着手指，你就能感觉到眼睛在转动。这个转动就是双眼深度线索中的物体部分线索，人的大脑能感知到眼睛转动了多少。另外，两个眼睛都会在**视网膜(retina)**上投影下图像，这两个图像至少有一处是相同的：视网膜中央。双眼在这儿都会显示两条视线相交之处的物体。其他的物体在视网膜上的位置不一致，我们的大脑能通过他们与被注视的物体的位置差别来计算出距离。
（同时参照 [这个](https://en.wikipedia.org/wiki/Binocular_disparity) 。）

### 聚焦(focus)：

聚焦线索的物理部分是**辐辏(accommodation)**，也就是调整眼睛的焦距；图像部分是当你在眼睛在变化焦距的时候，背景的**模糊程度**也在改变。

## VR电影中存在的问题



正如文章开头部分所说，现在的立体全景电影体验在实现视差上表现很差。接下来逐个列举。

### 聚焦：

这点上完全是错的。在现实中，如果你看着你的手指，你的眼睛就会聚焦在手指上；当你的手指移动，你的焦点也会跟着动。然而现在的HMD不是这样工作的。

现在的HMD多数是这个原理：有一个有效的聚焦距离。如下图所示，如果你的焦距是这个距离（聚焦在屏幕上），那么场景里所有的物体都是正确聚焦的。这样一来我们的两种线索都丢失了，我们不再调整眼睛的焦距，而改为固定焦距；我们也没有根据景深改变模糊程度。

![screenshot3](/assets/why_arent_vr_movies_immersive/3.png)

### 双眼视差：

双眼视差线索是有的，不过并不完善。如果你两眼**水平**直视前方，那可以得到正常的两眼视差；如果你歪着头，或者抬头、低头，双眼视差就完全错误了。然而观众在观影过程中抬头和低头是很难避免的。


### 单眼视差（移动视差）：

这是最严重的问题。在现在的VR电影中是没有移动视差的。当你在不改变面向的情况下移动你的头，画面不会有任何变化。

如果非要用数学来解释这个现象，可能是你观察的物体很大而且在无限远处，以致于你的任何运动相对它都不明显；然而其他的视角线索又告诉你它不在无限远处。那剩下的一个解释就是你观察的世界是在随你的头部运动的，你往左移动半米，世界也会跟着往左移动半米。这样会非常影响沉浸感 -- 世界像是一个包在你周围的2D绘画，而不是让你身处其中的三维世界。


## 一个可能的解决方案：观察阵和光场

既然问题的原因是缺少视差，那我们就来尝试捕捉和处理更多的视差信息。比如，我们可以把原来的观察点周围前后左右上下的一个区域内的视差信息都捕捉下来。这就是**观察阵(viewing volume)**。只要捕捉很小的一个观察阵，效果就会好很多。这样至少可以实现移动视差和全方位的双眼视差。而如果我们采样率够高、又有一个可以调节焦距的HMD，焦距线索也是可以实现的。

当然现况是这个解决方案还在实验阶段。

## 结束语

希望本文让你了解了改善VR电影的沉浸感需要克服的难点，也就是更好地实现视差。另外，除了光场，用3D引擎实时渲染也能提高沉浸感。

> 注：本文是从Lytro公司CTO Kurt Akeley于2016年12月2日在北京电影学院的演讲改编而来。Kurt近些年在进行光场捕捉设备的研究。
