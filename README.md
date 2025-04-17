# ergocub_suite
This repo contains all the sw dependencies and instructions needed by ergoCub robot.
It does not take into account any simulation environment.

| Software | Dependencies| Installation Place | Needed by demo |
|---------|--------------|----------------|-------------|
| [ergocub-gaze-control](https://github.com/hsp-iit/ergocub-gaze-control) | [yarp](https://github.com/robotology/yarp) [idyntree](https://github.com/robotology/idyntree) [ergocub-software(for URDF model)](https://github.com/icub-tech-iit/ergocub-software)| Robot Torso | Perception Demo |
| [ergocub-realsense-pose](https://github.com/hsp-iit/ergocub-realsense-pose) | [yarp](https://github.com/robotology/yarp) [idyntree](https://github.com/robotology/idyntree) [ergocub-software(for URDF model)](https://github.com/icub-tech-iit/ergocub-software) | Robot Torso | Perception Demo |
| [bt_nav2_ergocub](https://github.com/hsp-iit/bt_nav2_ergocub) | [Nav2](https://docs.nav2.org/) [yarp](https://github.com/robotology/yarp) | Laptop | Navigation Demo |
| [ergocub-rpc-interfaces](https://github.com/hsp-iit/ergocub-rpc-interfaces)| [yarp](https://github.com/robotology/yarp) | Robot Torso | Perception and Navigation Demo|
| [ergocub-bimanual](https://github.com/hsp-iit/ergocub-bimanual)| [yarp](https://github.com/robotology/yarp) [idyntree](https://github.com/robotology/idyntree) [Eigen](https://eigen.tuxfamily.org/index.php?title=Main_Page) [SimpleQPSolver](https://github.com/Woolfrey/SimpleQPSolver) [ergocub-software(for URDF model)](https://github.com/icub-tech-iit/ergocub-software)| Robot Torso | Integration and bimanual Demo |
| [ergocub_navigation](https://github.com/hsp-iit/ergocub_navigation)| [yarp](https://github.com/robotology/yarp) [Nav2](https://docs.nav2.org/) [bt_nav2_ergocub](https://github.com/hsp-iit/bt_nav2_ergocub) [pointcloud_to_laserscan](https://github.com/ros-perception/pointcloud_to_laserscan) [ergocub-software(for URDF model)](https://github.com/icub-tech-iit/ergocub-software)| Laptop | Navigation and Integration Demo |
| [ergocub-perception](https://github.com/hsp-iit/ergocub-perception) | [docker](https://www.docker.com/) [yarp](https://github.com/robotology/yarp) | Laptop | Perception and Navigation Demo |
| [ergocub-behavior](https://github.com/hsp-iit/ergocub-behavior) | [ergocub-rpc-interfaces](https://github.com/hsp-iit/ergocub-rpc-interfaces) [yarp](https://github.com/robotology/yarp) | Laptop | Perception and Navigation Demo |

## Demo Instructions
An extensive documentation on how to run the demo on `ergoCub` is present at [this link](https://github.com/hsp-iit/demos).
Take this as a general guide of the order of the steps that have to been made to run code on ergocub.

More importantly it is really helpful to understand how the hardware setup is made: [ergocub-robot-basics](https://github.com/hsp-iit/demos/blob/main/demos_instructions/ergoCub_Robot_Basics.md).

How to run the [full demo](https://github.com/hsp-iit/demos/blob/main/demos_instructions/ergoCub_Navigation_Perception_Demo.md).


## Software Organization

On each ergoCub robot is installed the [robotology-superbuild](https://github.com/robotology/robotology-superbuild) and it's kept at a certain tag for allowing a stable working platform. Once it gets updated on a robot, it is announced on the relative Teams Channel.

All the HSP code must be cloned inside the HSP folder (for each PC) in `/usr/local/src/robot/hsp`

## Aliases, Shortucts and Environment Variables

On the robot PCs are available some aliases or shortcuts that can come in handy to `cd` to important directories:

`gotoRobotsConfigurationFolder` brings you to your current robot configuration folder.

`goToBuildSuperbuild` if done inside a repo under the `src` folder of the superbuild, it takes you to its relative build folder (if present). Useful to build only the specific repo (by doing `cmake .` and `make install`).

`test-speaker` tests if the speakers are working.

Useful environment variables:

`YARP_ROBOT_NAME` environment variable that contains the robot name. Used to automatically select configurations files.

`ROBOTOLOGY_SUPERBUILD_INSTALL_DIR` is the path where the software has to be installed: `/usr/local/src/robot/robotology-superbuild/build/install`.

`ROBOT_CODE` is the parent directory where all the code repositories are contained: `/usr/local/src/robot`. Here you will find the folder `hsp` where you can clone your code.

