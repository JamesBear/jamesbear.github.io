---
layout: post
title:  "如何在HTC Vive中实现瞬移 -- 一个示例"
date:   2016-08-04 15:30:30 +0800
categories: vive tech
---

利用HTC Vive的定位系统，玩家可以自由的在虚拟场景中行走。可如果你的场景比一个房间大，就该考虑引进瞬移机制了。当玩家走到房间的边缘，已经不能再往前走时，用瞬移可以大幅度改变玩家所在位置——甚至朝向，让游戏得以继续。

## 什么是瞬移

瞬移(teleport)是对玩家在短时间内做远距离移动的行为。

最简单的瞬移机制就是记录手柄指向的位置，然后当trigger键按下时，将玩家移动到这个位置。本文将介绍一个不同的方案，不但能实现位置的变化，还能改变朝向。

## 举个例子

像下面这张图展示的一样：
游戏中手柄会射出一道激光，激光指向的地方有一个标记物体。有一个半透明的幻影一直在向标记物体跑去，标记位置变了，他就会转身朝新的位置跑去。任何时候，按下手柄的trigger键会使玩家的位置和朝向=幻影的位置和朝向。

![screenshot1](/assets/how_to_teleport_with_vive/1.png)

换句话说，如果你想去某个位置，就引导幻影跑过去，然后按下trigger即可。同理，如果你想要切换成某个视角，引导幻影的跑动和朝向，调好方向后按下trigger。

做成这种方式的理由是：
减少瞬移的频率，玩家需要先引导幻影移动到想要的位置
可以控制朝向
通过引导幻影，玩家可以瞬移到一些激光指不到的死角

例程的代码放在: https://github.com/JamesBear/vive_teleport_test

## 技术难点

想要移动玩家的位置，就需要改变游戏中camera物体的位置。而这个位置是受定位系统控制的。每帧SteamVR_TrackedObject（一个贴在camera和controller上的组件）都会获取新的玩家所戴的HMD在房间中的位置和朝向，然后将camera与之同步。所以仅仅设置camera.transform.position是不可以的——下一帧它就会被设置回去。

那如何做到移动玩家位置，并且不打断定位系统对camera的同步？答案是CameraRig。

![screenshot2](/assets/how_to_teleport_with_vive/2.png)

图中的camera (head)就是位置会和HMD同步的camera，它的代表了玩家在游戏中的位置。注意，他的父物体是CameraRig，通常情况下它的位置和朝向都是归零的，这种情况下camera (head)的位置=HMD的位置；而如果改变它的位置，玩家的位置就会在camera (head)的本地位置(local position)没有变，可绝对位置会受影响。比如把cameraRig的y设置为2，玩家就会感觉在空中两米的位置飘浮着。所以想要瞬移，只要改变CameraRig的位置和朝向即可。

## 如何实现

### 一般的瞬移程序:

玩家定位的位置是camera.localPosition，现在想要移动到targetPositon，在不考虑转向的情况下只要这么做：

     CameraRig.position = targetPosition - camera.localPosition;

原因是 camera的本地位置+父物体(CameraRig)的位置 = camera的绝对位置。所以要让camera的绝对位置=targetPosition，就需要能满足等式camera.localPosition + CameraRig.position = targetPosition。这个等式通过移项可以获得上面那行代码。

### 考虑旋转的瞬移:

有些程序像本文提到的例程一样，需要考虑旋转。也就是说camera在游戏中的朝向未必和HMD定位的朝向保持一致。那需要对上面代码做如下改变：
对父物体CameraRig的旋转也做出类似改变
设置旋转后再设置位置
改变相对位置到绝对位置的转换式

最终的代码是:

```
cameraRig.rotation = targetRotation * Quaternion .Inverse(camera.localRotation);

var relativePos = cameraRig.TransformPoint(camera.localPosition) - cameraRig.position;
cameraRig.position = targetPos - relativePos;
```

## 总结

最简单的实现瞬移的方式是，改变camera父物体的位置和朝向。另外为了减少眩晕感，可以加上镜头的淡入淡出。