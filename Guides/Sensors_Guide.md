# Sensors on ErgoCub
The main sensors used on ergoCub are the camera `Realsense D455` and the lidar `rplidarS2`. There is also a IMU on the head (used by the navigation) and one on the torso (used by the walking-controller).

The camera and lidar are launched together on the head by `yarprobotinterface --config sensors.xml`.

The IMUs are already launched when starting the robot, i.e. on the robot torso `yarprobotinterface --config ergocub.xml`

The configurations files for the yarp devices are present on the [robots-configuration repo](https://github.com/icub-tech-iit/robots-configuration) under the folder `/YARP_ROBOT_NAME/sensors`

## Realsense D455
Camera present on `ergocub-head` pc. Launched standalone by the command:
```
yarprobotinterface --config realsense2.xml
```

The path of the launch file is: `/usr/local/src/robot/robotology-superbuild/src/robots-configuration/realsense2.xml`

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
The path of the launch file is: `/usr/local/src/robot/robotology-superbuild/src/robots-configuration/sensors.xml`

### Configuration steps (on new Robot or PC)
0) ssh on the robot head `ssh ergocub-head` from the robot laptop.
1) Install [yarp-device-rplidar](https://github.com/robotology/yarp-device-rplidar) setting `-DCMAKE_INSTALL_PREFIX` on the `robotology-superbuild` install path (usually `/usr/local/src/robot/robotology-superbuild/build/install`)
