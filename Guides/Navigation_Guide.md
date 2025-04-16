# ErgoCub Navigation
The navigation stack needs the following components (in the `src` folder inside the `ros2_workspace`):
1) [ergocub_navigation](https://github.com/hsp-iit/ergocub_navigation).
2) [bt_nav2_ergocub](https://github.com/hsp-iit/bt_nav2_ergocub).
3) [pointcloud_to_laserscan](https://github.com/ros-perception/pointcloud_to_laserscan).

## Install
You have two main methods to install and use the ergocub_navigation stack:
1) Use the docker and follow the instructions on [this repo](https://github.com/SimoneMic/docker_ergocub/tree/ergoCubSN001). **This is the preferred method!**.
2) Setup from source by creating a `ros2_workspace` folder inside the `hsp` folder located in `/usr/local/src/robot/hsp`. ([ros2 workspace guide](https://docs.ros.org/en/iron/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html)). Then clone all the previously listed components under the `src` folder, and do `colcon build` in the workspace folder.

You have to add to the `.bashrc` the source of the ros2 env local setup by: `source /path/to/ros2_workspace/install/local_setup.sh`.

## Key Components
First read the guide on [how to run](https://github.com/hsp-iit/ergocub_navigation/blob/main/README.md) `ergocub_navigation`.

1) `planner_trigger_server` it synchronizes the Nav2 path planning with the double support phase of the [walking-controller](https://github.com/SimoneMic/walking-controllers/tree/nav_integration), via the `decorator_on_bool` in [bt_nav2_ergocub](https://github.com/hsp-iit/bt_nav2_ergocub), using a custom BT.
Run it by: `ros2 run ergocub_navigation planner_trigger_server`
It needs an already running `walking-controller` with the robot prepared and after the command `startWalking` has been given.
2) `ros2 launch ergocub_navigation launch_all.launch.py` launches the whole stack with the a) TFs publisher, b) the reference frame of the robot (`geometric_unicycle`), c) the odom publisher, d) lidar filtering, e) the nav2 stack and AMCL.
3) `path_converter` is a ros2 node that listen to the `/path` topic and transforms it in the robot frame and converts it to yarp for passing it to the walking controller. It is launched by the command: `ros2 launch ergocub_navigation path_converter.launch.py`.
4) `footprints_viewer` is a ros2 node that publishes the planned footsteps by the walking-controller as marker arrays. Used only by visualization purposes. Launched by: `ros2 launch ergocub_navigation footprints_viewer.launch.py`.
