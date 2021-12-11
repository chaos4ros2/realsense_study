realsense depth camera quick start：

### install package
```
// https://github.com/IntelRealSense/realsense-ros/tree/ros2#method-1-the-ros-distribution
sudo apt-get install ros-$ROS_DISTRO-realsense2-camera
```

### start camera node
```
// https://github.com/IntelRealSense/realsense-ros/tree/ros2#start-the-camera-node
ros2 launch realsense2_camera rs_launch.py
```

### point cloud
```
// https://chaosmamoru.slack.com/archives/C02FFPBSLLE/p1639231277003300
ros2 launch realsense2_camera demo_pointcloud_launch.py
```

Imageのtopic：/camera/color/image_raw
PointCloud2のtopic： /camera/depth/color/points

#### memo   
[tutorial](https://intel.github.io/robot_devkit_doc/pages/install.html) is for ubuntu18.04 
