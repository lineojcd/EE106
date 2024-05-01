# Install RPlidar A2 M8 for Kobuki base
Here are the steps to install RPlidar A2 M8 for Kobuki base on Ubuntu20.04 and ROS noetic :
```
cd ~/catkin_ws/src
git clone https://github.com/diegomrt/turtlebot_lidar.git
cd ..
catkin_make
source devel/setup.bash

```

# Mount RPLidar onto Kobuki base
In this file ~/catkin_ws/src/kobuki/kobuki_description/urdf/kobuki_standalone.urdf.xacro
Remove the previous content and use the below content:
```
<?xml version="1.0"?>
<robot name="kobuki_standalone"
       xmlns:sensor="http://playerstage.sourceforge.net/gazebo/xmlschema/#sensor"
       xmlns:controller="http://playerstage.sourceforge.net/gazebo/xmlschema/#controller"
       xmlns:interface="http://playerstage.sourceforge.net/gazebo/xmlschema/#interface"
       xmlns:xacro="http://ros.org/wiki/xacro">

  <!-- Defines the kobuki component tag. -->
  <xacro:include filename="$(find kobuki_description)/urdf/kobuki.urdf.xacro" />
  <xacro:include filename="$(find turtlebot_lidar)/urdf/rplidar.urdf.xacro"/>  
  <xacro:include filename="$(find turtlebot_lidar)/urdf/turtlebot_gazebo_rplidar.urdf.xacro"/>

  <xacro:kobuki/>
  <xacro:sensor_rplidar parent="base_link"/>
  <xacro:turtlebot_sim_2dsensor/>
</robot>

```

# Launch Kobuki in Gazebo
Here are the steps to install Kobuki base in Gazebo：
```
cd ~/catkin_ws/src
git clone https://github.com/yujinrobot/kobuki_desktop.git
cd kobuki_desktop/
rm -r kobuki_qtestsuite
cd ~/catkin_ws
catkin_make (or catkin build depends on your setting)
source ~/.bashrc

## Next, you can launch your Kobuki model in gazebo
roslaunch kobuki_gazebo kobuki_empty_world.launch
## In another terminal, run the below line to control the robot through keyboard controller
roslaunch kobuki_keyop keyop.launch
```

# Control Kobuki through Python API
Write your own code, and publish the Twist message through the topic "/mobile_base/commands/velocity" to move the Kobuki base. Before run your own code, do not forget to grant permission for your code by:
```
chmod 777 your_code_file_name.py
# Then run you code:
rosrun package_name your_code_file_name.py
```

# Run Kobuki base in Real world
In your first terminal, run the command below:
```
roslaunch kobuki_node minimal.launch
```
In your second terminal, run the command below:
```
roslaunch kobuki_keyop keyop.launch
```
This will allow you to control the Kobuki base through keyboard controller

