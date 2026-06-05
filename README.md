
<div align="center">
  <h1>🤖 TurtleBot3 Enhanced Navigation System</h1>
  <p><i>Autonomous A* Pathfinding & Dynamic Obstacle Avoidance in ROS Noetic</i></p>

  <img src="https://img.shields.io/badge/ROS-Noetic-22314E?style=for-the-badge&logo=ros&logoColor=white" alt="ROS Noetic" />
  <img src="https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/Ubuntu-20.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu" />
  <img src="https://img.shields.io/badge/Jupyter-F37626.svg?&style=for-the-badge&logo=Jupyter&logoColor=white" alt="Jupyter Notebook" />
</div>

<br />

A containerized deep-navigation stack for the TurtleBot3. This project bypasses the standard ROS `move_base` to implement a custom A* pathfinding algorithm coupled with real-time LiDAR processing for dynamic obstacle avoidance.

<!--
**Demonstration**
-->

## ✨ Key Features

* **Custom Path Planning 🗺️:** A* search algorithm executed across a 2D occupancy grid with line-of-sight path smoothing.
* **Dynamic Obstacle Avoidance 🛑:** Real-time LiDAR (`/scan`) processing. Halts forward momentum at a 0.5m threshold, calculates the widest clear gap, and reroutes dynamically.
* **Containerized Architecture 🐳:** Built on Kali Linux using a Distrobox Ubuntu 20.04 container, ensuring legacy ROS Noetic dependencies run flawlessly on a rolling-release host OS.
* **Jupyter Control Node 📓:** The simulation infrastructure (Gazebo/ROS Master) runs in native background terminals, while the control logic ("The Brain") executes sequentially via a Jupyter Notebook for maximum stability and debuggability.

## 🛠️ Technology Stack

### Core Technologies
* **Framework:** ROS Noetic
* **Simulation & Visualization:** Gazebo 3D, RViz
* **Languages:** Python 3.8
* **Math & Data:** NumPy, Matplotlib, `tf.transformations`

### Infrastructure
* **OS:** Kali Linux (Host) -> Ubuntu 20.04 (Container)
* **Virtualization:** Distrobox, Podman

---

## 🧠 How It Works

1.  **Map Processing:** Subscribes to the `/map` OccupancyGrid. Validates start and goal coordinates to ensure they fall within free space.
2.  **Path Generation:** Executes the A* search algorithm across the 2D pixel grid.
3.  **Path Smoothing:** Implements a line-of-sight algorithm to eliminate jagged grid movements, creating a direct, continuous trajectory.
4.  **Coordinate Translation:** Converts grid pixels back into real-world (X,Y) meters for the robot's odometry frame.
5.  **Control Loop (10Hz):** Continuously computes heading errors and dynamically adjusts linear/angular velocities based on real-time `/odom` feedback.

---

## 📋 Prerequisites

To run this project, you need an Ubuntu 20.04 environment (native or containerized) with the following packages installed:

```bash
sudo apt install ros-noetic-desktop-full ros-noetic-turtlebot3 ros-noetic-turtlebot3-simulations ros-noetic-turtlebot3-navigation python3-pip

```

Install the required Python dependencies:

```bash
pip3 install notebook==6.5.6 numpy matplotlib

```

---

## 🚀 Installation & Setup

1. **Clone the repository into your catkin workspace:**

```bash
cd ~/your_ws/src
git clone [https://github.com/Usama-Saif01/turtlebot3_enhanced_nav.git](https://github.com/Usama-Saif01/turtlebot3_enhanced_nav.git)

```

2. **Build the workspace:**

```bash
cd ~/your_ws
catkin_make
source devel/setup.bash

```

---

## 💻 Execution Guide

To ensure stability, the simulation engine and the Jupyter Notebook control node must be launched in separate terminals.

**1. Launch the Simulation Infrastructure (Terminal 1)**

```bash
export TURTLEBOT3_MODEL=waffle
roslaunch turtlebot3_gazebo turtlebot3_world.launch use_sim_time:=true

```

**2. Launch the Map Server (Terminal 2)**

```bash
rosrun map_server map_server "$(rospack find turtlebot3_navigation)/maps/map.yaml" _use_sim_time:=true

```

**3. Start the Jupyter Control Server (Terminal 3)**

```bash
cd ~/your_ws/src/turtlebot3_enhanced_nav/notebooks
jupyter notebook --no-browser

```

**4. Run the Code**
Connect your IDE (e.g., VS Code) to the generated Jupyter server URL. Execute the cells in `turtlebot3_astar_nav.ipynb` sequentially to calculate the path and initiate autonomous driving.

---

### 👨‍💻 Author

**Usama Saifullah**
*Information Technology Professional | Machine Learning & Cybersecurity* 🌐 [usamasaifullah.com] 🔗 [LinkedIn](https://www.google.com/search?q=https://www.linkedin.com/in/usama-saifullah-sethar)
