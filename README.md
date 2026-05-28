# MyRobot ROS 2 Stack

A complete, modular ROS 2 (Jazzy) autonomous robot stack for differential-drive robots. Designed simulation-first with Gazebo, online SLAM, Nav2 autonomous navigation, and vision processing.

<img src="docs/demo.gif" width="600" />

## 🚀 Features

- **Realistic Simulation:** Gazebo simulation with GPU lidar, IMU noise, and camera processing.
- **Hardware Agnostic:** Uses `ros2_control` for a seamless transition from simulation to real hardware.
- **Autonomous Navigation:** Mapping via `slam_toolbox`, localization via AMCL, and path planning via `Nav2`.
- **Vision Processing:** Aruco marker detection and camera calibration.

## 📸 Demo

| Step | Screenshot |
|------|------------|
| Initialization | <img src="docs/initialization.png" width="500" height="500"/> |
| Path Calculated | <img src="docs/new_path_calc.png" width="700" /> |
| Follow Path | <img src="docs/follow_path.png" width="500"  height="500" /> |
| Goal Reached | <img src="docs/goal_reached.png" width="500"  height="500" /> |

## 📋 Prerequisites

Tested on **Ubuntu 24.04** with **ROS 2 Jazzy**. 
Make sure you have installed the required ROS 2 dependencies:

```bash
sudo apt install ros-jazzy-navigation2 ros-jazzy-nav2-bringup \
                 ros-jazzy-slam-toolbox ros-jazzy-ros2-control \
                 ros-jazzy-ros2-controllers ros-jazzy-gazebo-ros-pkgs
```

## 🛠️ Installation & Build

Clone the repository into your ROS 2 workspace and build the stack:

```bash
mkdir -p ~/myrobot_ws/src
cd ~/myrobot_ws/src
git clone <your-repository-url> .

cd ~/myrobot_ws
colcon build --symlink-install
source install/setup.bash
```

## 🎮 Usage

### 1. Launching the Simulation Environment
To spin up the robot in the Gazebo `small_house` world:
```bash
ros2 launch myrobot_bringup sim_robot.launch.py
```
*(Optional)* To view the robot model without simulation: `ros2 launch myrobot_description display.launch.py`

### 2. Creating a Map (SLAM)
Launch the simulation with the SLAM argument enabled to create a map of the environment:
```bash
ros2 launch myrobot_bringup sim_robot.launch.py use_slam:=true
```

### 3. Autonomous Navigation (Nav2 & AMCL)
If you already have a map, launch the simulation defaulting to AMCL localization. You can then use the `2D Goal Pose` tool in RViz to command the robot:
```bash
ros2 launch myrobot_bringup sim_robot.launch.py use_slam:=false
```

## 📦 Package Overview

| Package | Description |
|---------|-------------|
| `myrobot_description` | URDF/xacro, Gazebo worlds, meshes, and RViz configs. |
| `myrobot_controller` | `ros2_control` configurations (DiffDrive, joint states). |
| `myrobot_bringup` | Centralized launch files for simulation and deployment. |
| `myrobot_mapping` | `slam_toolbox` configurations for online mapping. |
| `myrobot_localization` | AMCL global localization and EKF sensor fusion. |
| `myrobot_navigation` | Nav2 behavior trees, planners, controllers, and costmaps. |
| `myrobot_vision` | Camera configurations and OpenCV-based Aruco detection. |

## ⚙️ Configuration Hints
Most controller, navigation, and localization parameters are exposed in their respective package's `config/` directory. 
- **Robot Kinematics:** Adjust wheel separation (`0.30 m`) and radius (`0.033 m`) inside `myrobot_controller/config/myrobot_controllers.yaml`.
- **World Selection:** Pass `world_name:=<name>` to the bringup launch file to change the Gazebo environment.

## 🙏 Acknowledgments

This project builds upon great open-source frameworks:
- **[Articu-Bot](https://github.com/joshnewans/articubot_one.git)** by Josh Newans
- **[Bumper-Bot](https://github.com/AntoBrandi/Bumper-Bot.git)** by Antonio Brandi
- **[Nav2](https://github.com/Navigation2/nav2)** Navigation Stack

