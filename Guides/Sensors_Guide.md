# Sensors on ErgoCub
The main sensors used on ergoCub are the camera `Realsense D455` and the lidar `rplidarS2`. There is also a IMU on the head (used by the navigation) and one on the torso (used by the walking-controller).

The camera and lidar are launched together on the **head** by `yarprobotinterface --config sensors.xml`.

The IMUs are already launched when starting the robot, i.e. on the robot **torso** `yarprobotinterface --config ergocub.xml`

The configurations files for the yarp devices are present on the [robots-configuration repo](https://github.com/icub-tech-iit/robots-configuration) under the folder `/${YARP_ROBOT_NAME}/sensors`

## Realsense D455
Camera present on `ergocub-head` pc. Launched standalone by the command:
```
yarprobotinterface --config realsense2.xml
```

The path of the launch file is: `/usr/local/src/robot/robotology-superbuild/src/robots-configuration/${YARP_ROBOT_NAME}/realsense2.xml`

### Configuration steps (on new Robot or PC)
0) ssh on the robot head `ssh ergocub-head` from the robot laptop.
1) Update the realsense firmware (if necessary) to the latest release following [this guide](https://dev.intelrealsense.com/docs/firmware-update-tool)
2) Install [yarp-device-realsense2](https://github.com/robotology/yarp-device-realsense2) setting `-DCMAKE_INSTALL_PREFIX` on the `robotology-superbuild` install path (usually `/usr/local/src/robot/robotology-superbuild/build/install`)

*Already faced Issue* - Realsense stopping streaming black imgs after system/pc formatting/update: check if the fw of the realsense is up to date. If not, update it.

## RPlidarS2
LIDAR present on `ergocub-head` pc. Launched with the camera by the command:
```
yarprobotinterface --config sensors.xml
```
The path of the launch file is: `/usr/local/src/robot/robotology-superbuild/src/robots-configuration/${YARP_ROBOT_NAME}/sensors.xml`

### Configuration steps (on new Robot or PC)
0) ssh on the robot head `ssh ergocub-head` from the robot laptop.
1) Install [yarp-device-rplidar](https://github.com/robotology/yarp-device-rplidar) setting `-DCMAKE_INSTALL_PREFIX` on the `robotology-superbuild` install path (usually `/usr/local/src/robot/robotology-superbuild/build/install`)


## Bridge to ROS2

The sensors data is compressed and transmitted via wifi using YARP `robot->laptop`. Then on the laptop is converted to ros2 using [yarp-devices-ros2](https://github.com/robotology/yarp-devices-ros2).

You can find the configurations files to launch the conversion applications on [ergocub_navigation](https://github.com/hsp-iit/ergocub_navigation/tree/main/config/yarp) or on [robots-configuration](https://github.com/robotology/robots-configuration/tree/devel/ergoCubSN001/sensors/ros2_repeaters)

You can launch them by running the `yarprobotinterface --config <file>` (**NOTE: you will need ROS2 on the system to run these**):

`depth_compressed_ros2.xml` publishes the camera info: `/camera/rgbd/camera_info`  img: `/camera/rgbd/img` and depth: `/camera/rgbd/depth` topics on ros2, plus the colored pointcloud `/camera/depth/color/points`.

`head_imu_ros2.xml` publishes the imu message in ros2 format on the topic `/head_imu`.

`lidar_compressed_ros2.xml` publishes the lidar message in ros2 format on the topic `/scan_local`
