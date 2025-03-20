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
