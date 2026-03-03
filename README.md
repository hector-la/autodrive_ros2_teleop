AutoDRIVE - Installer & ROS 2 Bridge (AIROS) 🏎️
This repository provides a detailed guide to installing the AutoDRIVE simulator and configuring the communication bridge between the AutoDRIVE Simulator and ROS 2 Humble. The goal is to enable control and data acquisition for the F1TENTH vehicle for robotics and autonomous navigation projects.

📋 Prerequisites
Before starting, ensure your environment meets the following requirements:

Operating System: Ubuntu 22.04

Middleware: ROS 2 Humble

🕹️ 1. Simulator Installation
Follow these steps to prepare the simulation environment:

Download the compressed file (.zip) of the simulator for Linux.

⬇️ Download AutoDRIVE Simulator Linux

Save the file in an accessible location and unzip it (e.g., ~/Downloads/AutoDRIVE_Sim).

Open the terminal and navigate to the folder where you extracted the simulator.

Bash
cd ~/Downloads/AutoDRIVE_Sim
Grant execution permissions to the main binary:

Bash
chmod +x "AutoDRIVE Simulator.x86_64"
Start the simulator by running the following command:

Bash
./"AutoDRIVE Simulator.x86_64"
🛠️ 2. ROS 2 BRIDGE (DEVKIT)
2.1 Workspace and Virtual Environment Setup
To avoid conflicts between the library versions required by the Devkit and the system defaults (Ubuntu 22.04), we will use a Virtual Environment (venv). This isolates the Python dependencies within a controlled environment.

2.2 Create Workspace
Run the following commands to create the folder structure:

Bash
mkdir -p ~/autodrive_ws/src
cd ~/autodrive_ws
2.3 Configure the Virtual Environment (venv)
2.3.1 Create and activate the environment inside your workspace root:

Bash
# Create the environment
python3 -m venv venv

# Activate the environment (You must do this in every new terminal)
source venv/bin/activate
2.3.2 Install Dependencies
With the environment active (venv), install the necessary libraries using these verified versions for stability:

Bash
pip install eventlet==0.33.3 Flask-SocketIO==4.1.0 python-socketio==4.2.0 \
            python-engineio==3.13.0 gevent-websocket==0.10.1 \
            transforms3d attrdict numpy==1.23.5 opencv-contrib-python
2.3.3 Critical Compatibility Patch
The attrdict library has incompatibilities with collection modules in Python 3.10+. To fix the import error without affecting the global system, apply this patch directly to the files inside your venv:

Bash
# Fix Mapping import error in attrdict
find ~/autodrive_ws/venv/lib/python3.10/site-packages/attrdict/ -name "*.py" -exec sed -i 's/from collections import Mapping/from collections.abc import Mapping/g' {} +

# Fix multiple import variants
find ~/autodrive_ws/venv/lib/python3.10/site-packages/attrdict/ -name "*.py" -exec sed -i 's/from collections import Mapping, MutableMapping, Sequence/from collections.abc import Mapping, MutableMapping, Sequence/g' {} +
2.4 Cloning and Organizing the Workspace
To ensure ROS 2 compiles correctly, we must extract only the necessary metapackage from the official repository and remove redundant files.

Cloning the Repository:
Navigate to your workspace source folder and clone the Devkit:

Bash
cd ~/autodrive_ws/src
git clone --single-branch --branch AutoDRIVE-Devkit https://github.com/Tinker-Twins/AutoDRIVE.git
Extraction and Cleanup:
To avoid duplicate package errors during compilation, move the ROS 2 folder to the src root and delete the rest of the repository:

Bash
# Move the ROS 2 metapackage
mv ~/autodrive_ws/src/AutoDRIVE/ADSS\ Toolkit/autodrive_ros2 ~/autodrive_ws/src/

# Delete the original repository to avoid conflicts
rm -rf ~/autodrive_ws/src/AutoDRIVE
2.5 Compiling the Workspace
Once the src folder is organized, proceed to build the packages. Ensure the virtual environment is active so ROS recognizes the installed Python dependencies.

Bash
# Return to workspace root
cd ~/autodrive_ws

# Activate environment and load ROS 2
source venv/bin/activate
source /opt/ros/humble/setup.bash

# Compile
colcon build --symlink-install

# Source the newly compiled workspace configuration
source install/setup.bash
🚀 3. Execution and Teleoperation
To start the system, follow this order strictly. We recommend opening independent terminals for each process.

Step 1: Start the Communication Bridge
You have two modes depending on whether you need sensor visualization or want to save system resources:

Option A: Bridge with Visualization (RViz)
Use this to see the LiDAR, camera feed, and the vehicle's 3D model while driving.

Bash
cd ~/autodrive_ws
source venv/bin/activate
source /opt/ros/humble/setup.bash
source install/setup.bash
export PYTHONUNBUFFERED=1
# Launches the bridge and opens RViz automatically
ros2 launch autodrive_f1tenth simulator_bringup_rviz.launch.py
Option B: Simple Bridge (Headless)
Use this if you only want to teleoperate the car without the graphical overhead of RViz.

Bash
cd ~/autodrive_ws
source venv/bin/activate
source /opt/ros/humble/setup.bash
source install/setup.bash
export PYTHONUNBUFFERED=1
# Launches only the communication bridge
ros2 launch autodrive_f1tenth simulator_bringup_headless.launch.py
Step 2: Run the Simulator
Open a new terminal and launch the simulator:

Bash
cd ~/Downloads/AutoDRIVE_Sim  # Adjust path to your folder
./"AutoDRIVE Simulator.x86_64"
Actions in the simulator:

Click the antenna button (Disconnected) until it changes to Connected.

Verify that the driving mode is set to Autonomous.

Step 3: Keyboard Control (Teleoperation)
In a new terminal, run the node to control the car using keys:

Bash
cd ~/autodrive_ws
source venv/bin/activate
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 run autodrive_f1tenth teleop_keyboard
Controls: W (Accelerate), S (Reverse), A (Left), D (Right), X (Brake).

Note: You must keep this terminal window selected/focused for the vehicle to respond to keystrokes.
