# Spot Workspace

This workspace has been tested with ROS Melodic. And may work for ROS Kinetic. This directory contains all the code used for RRT based Path Planning.

## Dependencies
To install all the dependencies having this directory as the present working directory, use the command:
```bash
rosdep install --from-paths src --ignore-src -r -y
```

## Steps to Run

1. Download robot configs

```bash
cd /src/robots
./install_descriptions
```

2. Having this directory as the present working directory, build the project

```bash
catkin_make
```

3. Source the files

```bash
source devel/setup.bash
```

4. Start the Spot simulation, and keep the terminal running. The Gazebo window showing the Spot Robot in an environment would open.

```bash
roslaunch spot_config gazebo.launch
```
(Sometimes the spot would spawn too low; if that happens add 
```
    ...
    <arg name="world_init_z"       default="0.0" />
    ...
    <include file="$(find champ_gazebo)/launch/gazebo.launch">
        ...
        <arg name="world_init_z"       value="$(arg world_init_z)" />
        ...
    </include>    
```
to robots/configs/spot_config/launch/gazebo.launch), then launch the following:
```bash
roslaunch spot_config gazebo.launch world_init_z:=0.8
```

5. Keep the previous terminal running, and open a new terminal. Source the files, as given in 2. Then, start the Path Planning Node for Spot, and keep the terminal running. The RViz visualization window showing the robot in the same environment would show up.

```bash
roslaunch global_path_planning champ_world.launch rviz:=true
```

6. Keep the previous terminal running, and open a new terminal. Source the files, as given in 2. Then, start the Path Planning Server. To check if the code is running properly. Check the second terminal, in which the Path Planning Node is running. On correct execution, after some time, the log `odom received!` should be visible.

```bash
roslaunch global_path_planning path_planning_server.launch
```

7. In the RViz window, select the 2D Nav Goal button and select the goal position on the map. The orientation can be set while selecting the goal position and dragging the cursor. On setting the position, the visualization of the selected path planning algorithm would start.

8. By default the rrt algorithm would work. To set the path planning algorithm, out of rrt or prm. Go to the file `src/global_path_planning/src/path_planning_server.py`. Instead of calling the `rrt` function, call the function `prm` with the same arguments to run the respective algorithm.
