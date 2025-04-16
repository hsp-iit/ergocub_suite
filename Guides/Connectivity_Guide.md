# Connectivity Guide

This document explains how the connections of the middlewares are enstablished on the real robots.

Keep [the base guide](https://github.com/hsp-iit/demos/blob/main/demos_instructions/ergoCub_Robot_Basics.md) as a reference on how to phisically setup the cable connections with the hardware. (Read it, really, you need it)

Connection types:
1) By **default** the robot is connected via wifi to a router, depending on the robot you will see something like: `ergoCub_00X_Wifi`. This router is connected via ethernet to the laptop, where you will see a phisical connection something like: ``.

2) An alternative is to have the robot connected using only the ethernet. You have to simply configure yarp to run in the ethernet network: `yarp conf laptop_ethenet_IP 1000` (see table below for IPs). **Remember to switch back to the default configuration once you have finished.**

To SSH to a PC on the robot, or the laptop, just do `ssh <alias-name>`, where `<alias-name>` is either `ergocub-torso` or `ergocub-head`.
Note: the alias stands for: `ergocub@IP` i.e. `ssh ergocub@10.0.2.13` for logging in ergoCubSN001 head.

Here's the table with the robots IPs for **wifi** and for the **ethernet** network:

**ergoCubSN000**
| Alias | wifi IP | ethernet IP | location |
| ----- | --- |  ---  |------ |
|`ergocub-laptop` | `10.0.2.4` | `10.0.1.4` | Alienware laptop outside the robot |
|`ergocub-head` | `10.0.2.3` | `10.0.1.3` | Nvidia Xavier on the robot head |
|`ergocub-torso` | `10.0.2.2` | `10.0.1.2` | Internal PC in the robot torso |
| | `10.0.2.1` |   | External server for connectivity and network access management (same for all robots)|

**ergoCubSN001**
| Alias | wifi IP | ethernet IP | location |
| ----- | --- | --- | ------ |
|`ergocub-laptop` | `10.0.2.14` | `10.0.1.14` | Alienware laptop outside the robot |
|`ergocub-head` | `10.0.2.13` | `10.0.1.13` | Nvidia Xavier on the robot head |
|`ergocub-torso` | `10.0.2.12` | `10.0.1.12` | Internal PC in the robot torso |

**ergoCubSN002**
| Alias | wifi IP | ethernet IP | location |
| ----- | --- | --- | ------ |
|`ergocub-laptop` | `10.0.2.24` | `10.0.1.24` | Alienware laptop outside the robot |
|`ergocub-head` | `10.0.2.23` |  `10.0.1.23` |Nvidia Xavier on the robot head |
|`ergocub-torso` | `10.0.2.22` | `10.0.1.22` | Internal PC in the robot torso |

## YARP

Yarp is the main middleware used by the robot, it lauches all the devices and opens the ports to comunicate with the motors and sensors of the robot.
To follow on with this guide you need to know the basics of yarp that are linked [here](https://www.yarp.it/latest/index.html#yarp_basics_usage). 
You REALLY need to know these things specifically (but are not sufficient):  [1-  YARP companion basics](https://www.yarp.it/latest/companion_use.html)   [2- Yarp Server](https://www.yarp.it/latest/group__yarpserver.html) [3- A network of ports](https://www.yarp.it/latest/note_ports.html) [4- Yarprobotinterface](https://www.yarp.it/latest/group__yarprobotinterface.html)

Once you have grasped the theoretical principles, you can carry on.

To configure the yarp network you have to type the following command on each robot:
```
yarp conf ergocub-laptop-IP 10000
```

Where `ergocub-laptop-IP` is either: `10.0.2.4` `10.0.2.14` `10.0.2.24` depending on the robot, if in wifi. Otherwise, if you are using the ethernet, you run in the subnetwork 1, instead of 2. (look at the table before)

The `yarpserver` runs **ALWAYS** on the laptop of the robot. Launch it using the `yarpmanager` as indicated in [the base guide](https://github.com/hsp-iit/demos/blob/main/demos_instructions/ergoCub_Robot_Basics.md)

To test if the yarp network is available on a PC do: 
```
yarp detect
```

## ROS2

Ros2 is used mainly for the navigation stack.
The version of Ros2 actually supported is `iron`, since the ubuntu version on the systems is 22.04.

Ros2 is installed on the robot torso for all the robots. If you need to install it, follow [the official guide](https://docs.ros.org/en/iron/Installation.html).
Then it is **MANDATORY** to install cyclonedds following [the guide](https://docs.ros.org/en/iron/Installation/DDS-Implementations/Working-with-Eclipse-CycloneDDS.html).

You also have to source the ROS2 setup on the `.bashrc` in home by: `source /opt/ros/iron/setup.bash`

On the laptop instead we use docker build using [this branch from this repo](https://github.com/SimoneMic/docker_ergocub/tree/ergoCubSN001) use the scripts `build_docker.sh` and `run.sh` to build (do it once) or to run it.

The docker image available are two (for SN001 and 002): `simonemiche/ergocub_nav_base:ergocubSN001` `simonemiche/ergocub_nav_base:ergocubSN002`

On ergocubSN000 everything is installed on the laptop, so no docker is required. However it is still supported in the dockerfile, so it is possible to build the docker for SN000 and use it.

On the head we don't use ROS2, but only YARP.

### Cyclone configuration

The connectivity configuration for ROS2 on the dockers is managed automatically based on the environment variable `YARP_ROBOT_NAME`.
The `run.sh` script loads at launch time the correct config file.

Anyway, for clarity sake, here's how the ROS2 configuration is handled on the robot:
On each PC in the network (torso and laptop/docker) we have configured cyclone by setting the following environment variables (on the `.bashrc`).

```
RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
```

```
CYCLONEDDS_URI=/path/to/cyclonedds.xml
```
The configuration file `cyclonedds.xml` contains how the DDS handles the comunications of the nodes. In our case we don't use the `multicast` connectivity but by `peer`. This means that we specify the IPs of the network interfaces that are on the network, i.e. who speaks to who.

Example for ergoCubSN002 laptop/docker (comments have been added for explainatory purpose):
```
<?xml version="1.0" encoding="UTF-8" ?>
<CycloneDDS xmlns="https://cdds.io/config" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://cdds.io/config https://raw.githubusercontent.com/eclipse-cyclonedds/cyclonedds/master/etc/cyclonedds.xsd">
   <Domain Id="any">
      <General>
         <AllowMulticast>false</AllowMulticast>
         <Interfaces>
            <NetworkInterface address="10.0.2.24" priority="default"/>
         </Interfaces>
      </General>
      <Discovery>
         <MaxAutoParticipantIndex>300</MaxAutoParticipantIndex>
         <ParticipantIndex>auto</ParticipantIndex>
         <Peers>
            <Peer address="10.0.2.24"/>  <!-- Laptop IP -->
            <Peer address="10.0.2.22"/>  <!-- Robot torso -->
            <Peer address="10.0.2.123"/> <!-- "external IP"-->
         </Peers>
      </Discovery>
   </Domain>
</CycloneDDS>
```
Key aspects:
1) Under `<Interfaces>` We have to indicate the IP of the machine on the network.
2) In `<MaxAutoParticipantIndex>` we say what's the maximum number of ros2 nodes that can be present on the network. 300 is quite high number, if you put this too low, like 30, you risk incurring in some ros2 applications not starting, having some strange errors of the DDS that is unable to start some nodes. So it is crucial to have an high threshold.
3) Under `<Peers>` we list the IPs of the other machines on the network to comunicate with. To be able to discover topics and nodes.

On the robot torso we have the same config file but changing the Laptop IP on `NetworkInterface` with the one of the torso.

There is a "jolly" IP `10.0.2.123` that one can use to connect with his personal laptop to join the network. You can do this by connecting your laptop via ethernet to the router.



