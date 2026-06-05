

```markdown
# TurtleBot3 Enhanced Navigation: A* Pathfinding & Dynamic Obstacle Avoidance

An autonomous navigation stack for the TurtleBot3, engineered using ROS Noetic and Python. This project bypasses standard ROS navigation (move_base) to implement a custom A* pathfinding algorithm coupled with real-time LiDAR processing for dynamic obstacle avoidance. 

> **Demonstration**
> ![TurtleBot3 Navigation Demo](https://github.com/user-attachments/assets/YOUR-GIF-LINK-HERE)

## 🏗️ Architecture & Engineering

This project is built on a highly containerized architecture designed to run legacy ROS environments safely on rolling-release distributions.

* **Containerized Environment:** Executed within an isolated Ubuntu 20.04 Distrobox container running on a Kali Linux host system.
* **Separation of Concerns:** The simulation infrastructure (Gazebo/ROS Master) runs in native terminal background processes, while the control logic ("The Brain") is executed via a connected Jupyter Notebook server to ensure a stable compute environment and easy debugging.
* **Control Loop:** Operates at 10Hz, computing heading errors and dynamically adjusting linear/angular velocities based on real-time `/odom` and `/scan` feedback.

## 🛠️ Technology Stack

* **Core:** ROS Noetic, Gazebo 3D Simulator, RViz
* **Logic:** Python 3.8, Jupyter Notebook (v6.5.6)
* **Math & Data:** NumPy, Matplotlib, `tf.transformations`
* **Infrastructure:** Kali Linux, Distrobox, Podman

## 🧠 How It Works

1. **Map Processing:** Subscribes to the `/map` OccupancyGrid. Validates start and goal coordinates to ensure they fall within free space.
2. **Path Generation:** Executes a custom A* search algorithm across the 2D pixel grid.
3. **Path Smoothing:** Implements a line-of-sight algorithm to eliminate jagged grid movements, creating a direct, continuous trajectory.
4. **Coordinate Translation:** Converts grid pixels back into real-world (X,Y) meters for the robot's odometry frame.
5. **Dynamic Avoidance:** Monitors the front 90-degree FOV via the `/scan` LiDAR topic. If an object breaches the safety threshold (0.5m), the robot halts forward momentum, calculates the widest available clear gap, and rotates to bypass the obstacle before resuming its A* trajectory.

## 📋 Prerequisites

To run this project, you need an Ubuntu 20.04 environment (native or containerized) with the following installed:

* `ros-noetic-desktop-full`
* `ros-noetic-turtlebot3`
* `ros-noetic-turtlebot3-simulations`
* `ros-noetic-turtlebot3-navigation`
* `python3-pip`

Install the required Python dependencies:
```bash
pip3 install notebook==6.5.6 numpy matplotlib

```

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

## 💻 Execution Guide

To ensure stability, the simulation and the Jupyter Notebook must be launched separately.

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
Connect your local IDE (like VS Code) to the Jupyter server URL. Execute the cells in `turtlebot3_astar_nav.ipynb` sequentially to calculate the path and initiate autonomous driving.

---

### Author

**Usama Saifullah** Information Technology Professional | Cybersecurity & Robotics

🌐 [usamasaifullah.com](https://www.google.com/search?q=https://usamasaifullah.com) | 🔗 [LinkedIn](https://www.google.com/search?q=https://www.linkedin.com/in/usamasaifullah)

```

```
