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
Then go to this file: ~/catkin_ws/src/turtlebot_lidar/urdf/rplidar.urdf.xacro
Remove the previous content and use the below content:
```
<?xml version="1.0"?>
    <!-- script_version=1.1 -->
    <robot name="sensor_rplidar" xmlns:xacro="http://ros.org/wiki/xacro">
      <xacro:include filename="$(find turtlebot_lidar)/urdf/turtlebot_gazebo_rplidar.urdf.xacro"/>
      <xacro:include filename="$(find turtlebot_lidar)/urdf/turtlebot_properties.urdf.xacro"/>

<!-- rplidar Laser -->
<xacro:macro name="sensor_rplidar" params="parent">
  <joint name="laser" type="fixed">
     <axis xyz="0 1 0" />
     <origin xyz="0 0 0.160" rpy="0 0 3.14159" />
     <parent link="base_link" />
     <child link="rplidar_link" />
  </joint>

  <link name="rplidar_link">
    <collision>
      <origin xyz="0 0 0" rpy="1.5707 0 1.5707"/>
      <geometry>
        <mesh filename="package://turtlebot_lidar/meshes/rplidar.dae" scale="0.001 0.001 0.001" />
      </geometry>
    </collision>
    <visual>
      <origin xyz="0 0 -0.03" rpy="1.5707 0 4.71229"/>
      <geometry>
        <mesh filename="package://turtlebot_lidar/meshes/rplidar.dae" scale="0.001 0.001 0.001" />
      </geometry>
    </visual>
    <inertial>
      <mass value="1e-5" />
      <origin xyz="0 0 0" rpy="1.5707 0 1.5707"/>
      <inertia ixx="0" ixy="0" ixz="0" iyy="0" iyz="0" izz="0" />
    </inertial>
  </link>
  
      <!-- Set up laser gazebo details -->
      <turtlebot_sim_2dsensor/>
      </xacro:macro>
</robot>
```

# Launch Kobuki with RPLidar in Gazebo
After the above configurations, to launch the Kobuki with RPLidar A2, run:
```
roslaunch kobuki_gazebo kobuki_empty_world.launch
```
You should be able to see the Kobuki with RPLidar active in the empty Gazebo world now~

When the Kobuki and RPLidar is running in Gazebo, you can type the below command in your terminal:
```
rostopic list
# If you see the '/scan' topic, you can run the below code to subscribe this topic and monitor the output 
rostopic echo /scan
```

# Control Kobuki through Python API
Write your own code, and publish the Twist message through the topic "/mobile_base/commands/velocity" to move the Kobuki base. Before run your own code, do not forget to grant permission for your code by:
```
chmod 777 your_code_file_name.py
# Then run you code:
rosrun package_name your_code_file_name.py
```

# Run RPLidar A2 M8 in Real world
When you have real RPLidar and want to run it in the real world, you can run the command below:
```
cd ~/catkin_ws/src
git clone https://github.com/Slamtec/rplidar_ros.git
cd ..
catkin_make (or catkin build rplidar_ros depends on your setting)
source devel/setup.bash

# Connect your RPLidar A2 M8 to your laptop
roslaunch rplidar_ros view_rplidar_a2m8.launch
```
Then you will be able to visualize the LaserScan in Rviz
