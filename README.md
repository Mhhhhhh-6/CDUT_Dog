# CDUT_Dog

CDUT 四足机器人仿真项目 - ROBOCON 2026 仿生足式机器人挑战赛

## 环境要求

- Ubuntu 22.04
- ROS2 Humble

## 安装步骤

### 1. 安装依赖

```bash
sudo apt update
sudo apt install -y ros-humble-gazebo-ros-pkgs ros-humble-gazebo-ros2-control ros-humble-ros2-controllers
ros-humble-xacro ros-humble-robot-state-publisher ros-humble-controller-manager

2. 克隆项目

git clone -b feat/obstacle-course-map https://github.com/Mhhhhhh-6/CDUT_Dog.git
cd CDUT_Dog

3. 编译

source /opt/ros/humble/setup.bash
colcon build

启动仿真（机器狗 + 障碍赛地图）

source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch dog_bringup sim.launch.py

启动后 Gazebo 会加载 ROBOCON 2026 障碍赛场地和四足机器人。虚拟机首次启动较慢，请耐心等待。

查看关节状态

新开终端：

source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 topic echo /joint_states --once

发送关节力矩命令

ros2 topic pub /effort_controller/commands std_msgs/msg/Float64MultiArray "data: [0,0,0, 0,0,0, 0,0,0, 0,0,0]" --once

12个值分别对应 FL、FR、HL、HR 四条腿各3个关节（HipX、HipY、Knee）的力矩。

项目结构

CDUT_Dog/
└── src/
    ├── dog_bringup/          # 启动与配置
    │   ├── launch/           # 启动文件
    │   ├── urdf/             # 机器人 URDF/Xacro
    │   ├── meshes/           # 模型网格文件
    │   │   └── field/        # 障碍赛场地 STL
    │   ├── worlds/           # Gazebo 世界文件
    │   └── config/           # 控制器配置
    ├── dog_controllers/      # 控制器
    ├── dog_hardware/         # 硬件接口
    ├── dog_estimation/       # 状态估计
    ├── dog_common/           # 公共库
    └── control/
        └── single_leg_hop/   # 单腿跳跃控制

障碍赛场地说明

场地模型基于 https://github.com/Ruixi-Cheng/26RC_Field 项目，包含 ROBOCON 2026
障碍赛全部障碍物：直角绕杆、砂砾碎木坑、限高杆、大斜坡、木桥A/B、T字形台阶、高墙。
