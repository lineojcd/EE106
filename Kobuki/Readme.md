# Install Kobuki base
Here are the steps to install Kobuki base on Ubuntu20.04 and ROS noetic :
```
sudo apt-get install ros-noetic-kobuki-core
sudo apt install liborocos-kdl-dev
sudo apt-get install ros-noetic-ecl-*
sudo apt-get install libusb-dev
sudo apt-get install libftdi-dev
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
git clone https://github.com/yujinrobot/kobuki.git
git clone https://github.com/yujinrobot/yujin_ocs.git
git clone https://github.com/yujinrobot/kobuki_msgs.git
git clone https://github.com/yujinrobot/kobuki_core.git
cd yujin_ocs
mkdir save 
mv yocs_cmd_vel_mux save
mv yocs_controllers save
mv yocs_velocity_smoother save
rm -rf yocs*
cd save 
mv * ..
cd .. && rmdir save
cd ~/catkin_ws/
catkin_make
rosrun kobuki_ftdi create_udev_rules
```

# Launch Kobuki in Rviz (Optional)
```
roslaunch kobuki_description view_model.launch
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

# Launch Kobuki with RPLidar in Gazebo
Please check [here](https://github.com/lineojcd/EE106/tree/main/RPLidar).

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

