# 利用T265相机进行无人机室内定位 

## 设置px4飞控

利用QGroundcontrol对px4进行参数设置。

在`EKF2_AID_MASK`中勾选`vision position fusion`和`vision yaw fusion`使得`EKF2_AID_MASK`的值为**24**。

设置`EKF2_EV_DELAY`为**20**ms。

设置`EKF2_HGT_MODE`为**VISION**

在遥控器中设置飞行模式，利用拨动开关切换，视觉定位的飞行模式为**位置模式**

## 利用mavros建立计算机与px4的通讯

安装完mavros后，usb连接px4与tx2，终端运行：

`roslaunch mavros px4.launch`

启动：

`rostopic echo /mavros/state`

观察`connection`的状态是否为`True`

## 启动t265相机

安装完realsense驱动后，终端运行：

`roslaunch realsense_camera rs_t265.launch`

启动：

`rostopic echo /camera/odom/sample`

查看里程计输出

## 安装vision2mavros

进入catkin工作区间，编译vision_to_mavros包，示例：

```
cd catkin_ws/src
git clone https://github.com/thien94/vision_to_mavros
cd ..
catkin_make
```

编译完成后，**关闭所有其他结点，这条命令将启动所有节点**，直接运行无人机室内定位，终端运行：

`roslaunch vision_to_mavros t265_all_nodes.launch`

运行：

`rostopic echo /mavros/vision_pose/pose`

查看视觉融合位姿的结果

## 观察px4视觉融合

运行：

`rostopic echo /mavros/local_position/pose`

如果有非极小数的结果，则可以起飞。

为了简化操作，可以将以上launch文件合并为一个launch来运行所有节点。