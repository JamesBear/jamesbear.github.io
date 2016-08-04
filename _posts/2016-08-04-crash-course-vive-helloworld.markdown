---
layout: post
title:  "【Crash Course Vive】用Unity开发Vive的hello, world"
date:   2016-08-04 15:11:30 +0800
categories: crash course vive
---
大家好，我是HTC的James。这篇短文将告诉大家如何用Unity3D制作VR版的hello, world。

Starting from now ;) 

## 1. 开启SteamVR并连接Vive设备 

 (a) 登录Steam客户端，并点击右上角的VR按钮，这时会弹出SteamVR的小窗口 
        
![screenshot1](/assets/crash_course_vive_helloworld/1.png)
 
 
 (b) 连接好所有VR设备，连接成功后SteamVR窗口上的图标会全部变为绿色 
        
![screenshot2](/assets/crash_course_vive_helloworld/2.png)


## 2. 新建Unity3D工程 

![screenshot3](/assets/crash_course_vive_helloworld/3.png)


## 3. 通过Asset Store导入SteamVR Plugin 

![screenshot4](/assets/crash_course_vive_helloworld/4.png)


## 4. 拖入相关prefab 

先删除所有默认GameObject 

![screenshot5](/assets/crash_course_vive_helloworld/5.png)

然后将SteamVR/Prefabs中的所有prefab拖入Hierarchy窗口 

![screenshot6](/assets/crash_course_vive_helloworld/6.jpg)


## 5. 点击预览

这个时候Game窗口会提示你可以戴上头盔了。戴上后四处环视一下，就能找到控制器。 

![screenshot7](/assets/crash_course_vive_helloworld/7.png) 

好了，第一个程序就这么制作完成了。接下来大家就可以自行发挥啦~

导入好看的场景和模型，编写自己的gameplay。 

另外，大家可以参考SteamVR Plugin自带的示例场景，分别是： 

>    SteamVR/Scenes/example 
>
>    SteamVR/Extras/SteamVR_TestIK 
>
>    SteamVR/Extras/SteamVR_TestThrow 

这次就介绍到这里，之后我们会详细地介绍SteamVR各个脚本的应用。 

欢迎大家讨论。 