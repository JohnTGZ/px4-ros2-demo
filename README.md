# PX4-ROS2-Demo
This is a demo of offboard control with PX4 and ROS2.
PX4 and ROS2 is interfacing via XCRE micro-DDS.

## Requirements
- Ubuntu 22.04 Jammy
- ROS2 Humble. [Installation instructions](https://docs.ros.org/en/humble/Tutorials.html)
- QGroundControl. For setting the flight mode to offboard mode.
- VCSTool. For cloning upstream git repos. Can be installed using `sudo apt-get install python3-vcstool`

## Setup the workspace
```bash
mkdir -p ~/px4_ros2_ws/src
cd ~/px4_ros2_ws/src/
git clone https://github.com/JohnTGZ/px4-ros2-demo.git
vcs import  . < px4-ros2-demo/px4.repos
cd ~/px4_ros2_ws/
rosdep install --from-paths src --ignore-src --rosdistro humble -y -r
# Build the workspace
colcon build --packages-ignore px4
```

## Quickstart
3 terminals will be required to run the offboard demo. Make sure to source the workspace before running the examples

On the first terminal, launch the Gazebo SITL. This should also start a micrortps_client that will convert uORB messages into RTPS space.  
```bash
cd ~/px4_ros2_ws/src/upstream/px4-autopilot
make px4_sitl gazebo
```

On the second terminal, start the `micro-ros-agent` on UDP port 8888
```bash
cd ~/px4_ros2_ws/
micro-ros-agent udp4 --port 8888
```

On the third terminal, start the offboard control launch script. This script launches 3 different processes: A  
```bash
cd ~/px4_ros2_ws/
source ./install/setup.bash
ros2 launch px4_offboard offboard_position_control.launch.py
```
