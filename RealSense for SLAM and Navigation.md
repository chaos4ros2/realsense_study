# navigation2

[RealSense™ for SLAM and Navigation - Intel ROS Tutorial 0.3.0 documentation](https://intel.github.io/robot_devkit_doc/pages/rs_slam.html)

# **RealSense™ for SLAM and Navigation**

## **1. Overview**

cartographerを通じてのSLAMはロボットの位置推定にレーザースキャンデータを必要だ。Intel® RealSense™深度カメラ(D400シリーズ)は深度イメージを生成可能で、depthimage_to_laserscanパッケージを通じてレーザースキャンデータに変換できる、そしてt265カメラはオドメーターとして姿勢情報を提供することができる。そのため、RealSense™をSLAMやナビゲーションに利用する方法を提供する。

## **2. SLAM with RealSense™**

**依存パッケージをインストール**

```bash
source /opt/robot_devkit/robot_devkit_setup.bash
mkdir -p ~/ros2_ws/src && cd ~/ros2_ws/src
git clone https://github.com/ros-perception/depthimage_to_laserscan -b dashing-devel
cd .. && colcon build
source ~/ros2_ws/install/local_setup.bash
```

**Start to SLAM**

```bash
# In terminal 1, launch cartographer nodesource ~/ros2_ws/install/local_setup.bash
ros2 launch realsense_examples rs_cartographer.launch.py

# In terminal 2, launch Intel® RealSense™ D400 camera and T265 camera
# You should config the serial number and tf in the launch file ros2_intel_realsense/realsense_examples/launch/rs_t265_and_d400.launch.py before launch the camera
source /opt/robot_devkit/robot_devkit_setup.bash
ros2 launch realsense_examples rs_t265_and_d400.launch.py

# In terminal 3, launch the turtlebot3 for RealSense™ SLAM
export TURTLEBOT3_MODEL=waffle
source /opt/robot_devkit/robot_devkit_setup.bash
ros2 launch realsense_examples tb3_robot.launch.py

# In terminal 4, launch the teleoperation node for robot
source /opt/robot_devkit/robot_devkit_setup.bash
export TURTLEBOT3_MODEL=waffle
ros2 run turtlebot3_teleop teleop_keyboard
```

turtlebot3をキーボードで操作して動かし、マップを作成する。そしてマップ作成プロセスは終了すれば以下のコマンドでマップを保存してください：

```bash
# In terminal 5source /opt/robot_devkit/robot_devkit_setup.bash
source /opt/ros/dashing/local_setup.bash
ros2 run nav2_map_server map_saver -f ~/map
```

次に`map.pgm`を開いて内容を確認します。以下は`RealSense™` と`cartographer`で構築したマップとなる。

![Untitled](navigation2%200417880572f2428ea36f130adad3a173/Untitled.png)

## **3. Navigation with RealSense™**

一般的に、SLAM with RealSense™で作成したマップでナビゲーションを行うためには、ros2ナビゲーションスタックはビルドされ、使用可能になっているのが前提である。

**Bringup the turtlebot3**

```bash
# In terminal 1export TURTLEBOT3_MODEL=waffle
source /opt/robot_devkit/robot_devkit_setup.bash
ros2 launch realsense_examples tb3_robot.launch.py
```

**Start ROS2 realsense and depth image to laser scan**

```bash
# In terminal 2source /opt/robot_devkit/robot_devkit_setup.bash
source ~/ros2_ws/install/local_setup.bash
ros2 launch realsense_examples rs_nav.launch.p
```

**Start the navigation2 stack with the map**

```bash
# In terminal 3
export TURTLEBOT3_MODEL=waffle
source /opt/ros/dashing/local_setup.bash
ros2 launch nav2_bringup nav2_bringup_launch.py map:=$HOME/map.yaml

# In terminal 4
source /opt/ros/dashing/local_setup.bash
ros2 run rviz2 rviz2 -d $(ros2 pkg prefix nav2_bringup)/share/nav2_bringup/launch/nav2_default_view.rviz
```

最後、`RVIZ2`で初期姿勢とゴールを指定し、走行マップ上でturtlebot3に指示を出し、ナビゲーションする。

## **4. Known issues**

RealSense™を地面と平行に保持してください、傾きはSLAMに影響する恐れがある。