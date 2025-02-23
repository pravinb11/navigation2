# nav2_dynamic_bringup

The `nav2_dynamic_bringup` package is an example bringup system for launching carla to test navigation in dynamic environment.



```shell
ros2 launch nav2_bringup cloned_multi_tb3_simulation_launch.py robots:="robot1={x: 1.0, y: 1.0, yaw: 1.5707}; robot2={x: 1.0, y: 1.0, yaw: 1.5707}"
```

#### Unique

There are two robots including name and intitial pose are hard-coded in the launch script. Two separated unique robots are required params file (`nav2_multirobot_params_1.yaml`, `nav2_multirobot_params_2.yaml`) for each robot to bring up.

If you want to bringup more than two robots, you should modify the `unique_multi_tb3_simulation_launch.py` script.

```shell
ros2 launch nav2_bringup unique_multi_tb3_simulation_launch.py
```


#### Carla Simulation with carla ros bridge
To run carla simulation run the following script
```shell
sudo docker run --privileged --gpus all --net=host -e DISPLAY=:1 carlasim/carla:0.9.13 /bin/bash ./CarlaUE4.sh

```

To run the carla ros bridge, run the script
```
ros2 launch carla_ros_bridge carla_ros_bridge_with_example_ego_vehicle.launch.py
```

Then run the script
```
ros2 run carla_twist_to_control carla_twist_to_control
```

To generate traffic, run the script
```
cd /path_to_carla/CARLA_0.9.13/PythonAPI/examples/
python3 generate_traffic.py
```

#### Procedure for installation of Carla
Follow the link 
https://carla.readthedocs.io/en/docs-preview/build_docker/
 and run the script
```
docker pull carlasim/carla:0.9.13
```
Alternative is to download the carla from the following link
https://github.com/carla-simulator/carla/releases/tag/0.9.13 and download  CARLA_0.9.13.tar.gz
#### Procedure for installation of carla-ros-bridge

Download the .egg and .whl file from https://github.com/gezp/carla_ros/releases/tag/carla-0.9.13-ubuntu-22.04

clone the master branch of carla_ros_bridge from 
follow the instructions from https://carla.readthedocs.io/projects/ros-bridge/en/latest/ros_installation_ros2/

```
mkdir -p ~/carla-ros-bridge && cd ~/carla-ros-bridge

git clone --recurse-submodules https://github.com/carla-simulator/ros-bridge.git src/ros-bridge

rosdep update
rosdep install --from-paths src --ignore-src -r

colcon build

    export CARLA_ROOT=<path-to-carla>
    export PYTHONPATH=$PYTHONPATH:$CARLA_ROOT/PythonAPI/carla/dist/carla-<carla_version_and_arch>.egg:$CARLA_ROOT/PythonAPI/carla

    source ./install/setup.bash


    ros2 launch carla_ros_bridge carla_ros_bridge_with_example_ego_vehicle.launch.py

```