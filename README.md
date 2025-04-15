# Build Truck & Trailer CAD (Solidworks) Modelling and ROS2 Gazebo Simulation
Project: ENPM662 Introduction to Modelling

![Truck and Trailer Control](https://github.com/user-attachments/assets/6f86a237-87d5-42a3-954f-32c7ab3dd9ce)


* Custom build your own Truck and Trailer model using Solidworks CAD Software
* Attach frames and collisions for URDF compatibility
* Simulate using ROS2, Gazebo
* Deply RWD motion control
* Perform closed-loop feed back control

## Contributors:
* Hamsaavarthan Ravichandar
* Robens Midy Cyprien
* Kent-Diens Joseph
* Jonathan Leonard Crespo


## General Setup
-Verify you have ROS2 galatic distribution installed and also CMAKE necessary installations. Refer to: https://docs.ros.org/en/galactic/Installation/Ubuntu-Install-Debians.html
On command line run:
```sh
echo $ROS_DISTRO
```
-Install previously the following packages and any additional in your linux distribution running on the terminal the command:
```sh 
sudo apt install python3-colcon-common-extensions
```
-Install all necessary dependencies and libraries with pip for insrtance. Recommended to run in your own python environment.

## Steps to build and run project.

### Create the workspace and install all dependencies priorly

Copy the folder workspace **ROS_SRC_PKG** in your linux system, locate them via the terminal using cd syntax and run 
```sh
cd ~\PATH TO ROS_SRC_PKG
rosdep install -i --from-path src --rosdistro galactic -y
```

Build and source odometry and project1_pkg:

```sh
colcon build --packages-select project1_pkg
```
project1_pkg contains the scripts for do teleoperation and for establish trajectories to follow with
In case you got the following error create the include folder as follows 



```sh
mkdir -p ~/src/project1_pkg/include/project1_pkg
```
And run again the build command

```sh
colcon build --packages-select odometry
```
Odometry package is needed to track robot position and orientation as it contains plugin features.

Source ROS (Package will be identified) However you can do make this default when opening the terminal by modifying the .bashrc file. <ins>Don't forget your user password to give permissions </ins>
```sh
sudo nano ~/.bashrc
```

```sh
 source /opt/ros/galactic/setup.bash
```
**Source package to be identified**. Do this for every new terminal you open locating the Workspace folder:

```sh
source install/setup.bash
```

Before launch also run the following commands to install the controller dependencies:

```sh
sudo apt install ros-galactic-ros2-control ros-galactic-ros2-controllers ros-galactic-gazebo-ros2-control
sudo apt-get install ros-galactic-controller-manager
```
### Gazebo

Then launch package using command 

```sh
ros2 launch project1_pkg gazebo.launch.py
```

This will display an empty world with the robot at position (0,0,0)

![Screenshot 2024-11-02 005130](https://github.com/user-attachments/assets/a49a3164-baac-4654-b04e-39a9e7d9c5c8)

### RVIZ

To launch rviz open a new terminal, source the package and run: 

```sh
ros2 launch project1_pkg display.launch.py
```
![image](https://github.com/user-attachments/assets/e9b2be9f-8e9b-4574-9cf5-dc51c4454369)

To visualize lidar scanner run before in other terminal the gazebo simulation and after launch RVIZ again and use the ** Add** button and look for select the ** laser scanner** plugin 

![Screenshot 2024-11-03 003739](https://github.com/user-attachments/assets/a8808ea6-1277-43b1-a6c6-f9343e9e5ea2)

After check for the scan topic and it should be visible the lidar.

![image](https://github.com/user-attachments/assets/06a107e9-b038-4e6e-a073-657293a8ef4a)


## Closed Loop Controller
To launch the closed loop controller:

Be sure to open the gazebo empty world by following the previous steps to open it. 

Open new terminal,source package and run:

```sh 
ros2 run project1_pkg autonomous_control.py
```

The robot will now move from (0,0) to (10, 10) in a straight line

![image](https://github.com/user-attachments/assets/ecd65f1b-7c26-4be5-bf60-42fad1c6312f)


To launch subscriber to monitor robot position:
Open new terminal,source package and run:

```sh 
ros2 run project1_pkg odom_subscriber.py
```
This will give information of the current position of the robot 

If odometry was settled correctly you can visualize it via the topic **/odom**, using programs as rqt_plot or plotjugger. For more information refer to the following useful link tutorial .All credits to the author https://www.youtube.com/watch?v=MnMGjvYxlUk
You can also run if installed
```sh 
ros2 run rqt_plot rqt_plot
```

## Teleop:
There are two maps for teleop: competition_arena and competition_track. 

By default our package opens with the empty world. To launch the competition arena,
open the gazebo.launch.py in project1_pkg/launch. Uncomment the map for teleop section
in line 24 to 29. And comment out the empty world section from line 17 to line 22.

![image](https://github.com/user-attachments/assets/5e0b0c11-1796-448f-8890-f291092abf39)

Close any previously running gazebo terminals then

Build and source project1_pkg then launch gazebo with the command:

```sh
  colcon build --packages-select project1_pkg
```

```sh
  ros2 launch project1_pkg gazebo.launch.py
```

The following map should be displayed 
![Screenshot 2024-11-02 220423](https://github.com/user-attachments/assets/c0ef5b14-86e0-4e7c-9343-124d5dfa01fa)

Open new terminal and run:

```sh
  ros2 run project1_pkg teleop.py
```

Robot can now be controlled with W, A, S, D with W and S increasing and decreasing velocity 
respectively and A and D turning the wheel left and right respectively. 

To run the competition track, first close any running gazebo terminal then navigate to 
spawn_robot_ros2.launch.py in project1_pkg/launch. 

We need change the position so that the robot
spawns at the checkpoint on the track. Uncomment the position and orientation for competition lines
then comment the position and orientation for autonomous mode lines. 

![image](https://github.com/user-attachments/assets/ba658518-ba93-40c5-a2db-02c158fc7065)


Save, build, and source project1_pkg. 

```sh
colcon build --packages-select project1_pkg
```

Source the package 
```sh
source install/setup.bash
```

The competition track can then be run with the command:

```sh
ros2 launch project1_pkg competition.launch.py
```
![Screenshot 2024-11-02 012844](https://github.com/user-attachments/assets/f9b8efd5-510a-4f71-8e3a-ecff5c393554)

Then open new terminal,source package and run:

```sh
ros2 run project1_pkg teleop.py
```

![image](https://github.com/user-attachments/assets/b706db00-428b-44a5-9907-552f7effa192)


You should now be able to control the robot around the track with W, A, S, D

# Links
## Teleoperation demo
https://drive.google.com/file/d/1amAvnNG7gteSBgnBY4UivcKHt8HglslF/view?usp=sharing
## Autonomous mode demo
https://drive.google.com/file/d/1GjcDvZHY8bReXl_MzDkvWvlRrHpu3n1f/view?usp=sharing
