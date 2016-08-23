---
layout: post
title:  "HTC Vive Developer FAQ - Unity"
date:   2016-08-23 11:20:30 +0800
categories: vive tech
---

### 如何获得手柄(类型为SteamVR_Controller.Device)对象？

有两种方式。

#### 通过监听device_connected事件

1. 在测试脚本的OnEnable中写上

```    
SteamVR_Utils .Event .Listen( "device_connected", OnDeviceConnected);
```
        
2. 实现OnDeviceConnected函数

```    
private void OnDeviceConnected( params object [] args)
{
    var index = (int )args[0];

    var vr = SteamVR .instance;
    if (vr.hmd.GetTrackedDeviceClass((uint )index) != ETrackedDeviceClass.Controller)
        return ;

    var connected = (bool )args[1];
    if (connected)
    {
        controllerIndex = index;
    }
}
```

3. 查询设备对象

```
controller = SteamVR_Controller.Input(controllerIndex);
```
  
#### 通过SteamVR_Controller.GetDeviceIndex

```
int index = SteamVR_Controller .GetDeviceIndex(SteamVR_Controller . DeviceRelation.Leftmost);
controller = SteamVR_Controller.Input(controllerIndex);
```
          
#### 利用手柄GameObject

在[CameraRig]/Controller (left)上附加脚本，执行如下操作：

```
var controllerIndex = GetComponent<SteamVR_TrackedObject>().index;
if (controllerIndex != SteamVR_TrackedObject.EIndex.None)
    controller = SteamVR_Controller.Input((int)controllerIndex);
```

### 手柄扳机的API，包括扣动扳机，扣动的幅度。

如何得知当前trigger被扳动了多少？

```
float triggerAxis = controller.GetAxis(EVRButtonId.k_EButton_SteamVR_Trigger).x;
```

trigger按下事件

```
if (controller.GetPressUp(EVRButtonId .k_EButton_SteamVR_Trigger))
    DoSomething();
```

### 手柄侧面2个按钮(抓紧)的API

```
if (controller.GetPressUp(EVRButtonId .k_EButton_Grip))
    DoSomething();
```

### 正面最上面(菜单键)的API

```
if (controller.GetPressUp(EVRButtonId .k_EButton_ApplicationMenu))
    DoSomething();
```

### 圆盘按下API，触摸点坐标

圆盘键按键事件

```
if (controller.GetPressUp(EVRButtonId .k_EButton_SteamVR_Touchpad))
    DoSomething();
```

触摸点坐标

```
var touchPos = controller.GetAxis(EVRButtonId.k_EButton_SteamVR_Touchpad);
```

### VR 是否连接成功的API

```
     if (SteamVR.active)
```


### controller是如何自动更新位置的？

通过脚本SteamVR_TrackedObject。这个脚本监听new_poses事件，并在事件响应中修改自己的位置。

### 如何获得按键的事件？

在Update中写SteamVR_Controller .Input(deviceIndex).GetPress(buttonId)

其中buttonId是以下之一:

```
EVRButtonId .k_EButton_ApplicationMenu,
EVRButtonId .k_EButton_Grip,
EVRButtonId .k_EButton_SteamVR_Touchpad,
EVRButtonId .k_EButton_SteamVR_Trigger
```

### 如何替换controller的模型？

1. 删除Controller (left)　//或者right
2. 新建模型。例如点击菜单GameObject->3D Object->Capsule，并将scale设置为(0.1, 0.1, 0.1)
3. 给新模型添加SteamVR_TrackedObject脚本
4. 将[CameraRig]的SteamVR_ControllerManager组件的left或者right的值设置为新模型对象


### 如何获得HMD位置？

因为Camera (head)的SteamVR_TrackedObject组件在追踪HMD的位置，所以直接用Camera (head)的位置即可。



### 如何判断touchpad的哪个区域（上下左右？）被点击了

```
if (SteamVR_Controller .Input(index).GetPressUp(EVRButtonId .k_EButton_SteamVR_Touchpad))
{
    var touchPos = SteamVR_Controller .Input(index).GetAxis(EVRButtonId.k_EButton_SteamVR_Touchpad);
    // 在此判断touchPos属于上下左右中的哪个区域
}
```

