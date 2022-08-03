##OS - Ubuntu 20.04
Followed http://wiki.ros.org/noetic/Installation/Ubuntu, did the work.

##ROS BASICS
ROS NOTES
Navigating the ROS Filesystem
    • rospack allows you to get information about packages. In this tutorial, we are only going to cover the find option, which returns the path to package.
Example:
$ rospack find roscpp
would return:
      YOUR_INSTALL_PATH/share/roscpp
      
    • roscd is part of the rosbash suite. It allows you to change directory (cd) directly to a 	package or a stack.
    • roscd log will take you to the folder where ROS stores log files. Note that if you have not run any ROS programs yet, this will yield an error saying that it does not yet exist.
    • rosls is part of the rosbash suite. It allows you to ls directly in a package by name rather than by absolute path.

You may have noticed a pattern with the naming of the ROS tools:
    • rospack = ros + pack(age)
    • roscd = ros + cd
    • rosls = ros + ls

##**Creating a ROS Package**
Description: This tutorial covers using roscreate-pkg or catkin to create a new package, and rospack to list package dependencies.
What makes up a catkin Package?
For a package to be considered a catkin package it must meet a few requirements:
    • The package must contain a catkin compliant package.xml file.
        ◦ That package.xml file provides meta information about the package.
    • The package must contain a CMakeLists.txt which uses catkin.
        ◦ If it is a catkin metapackage it must have the relevant boilerplate CMakeLists.txt file.
    • Each package must have its own folder
        ◦ This means no nested packages nor multiple packages sharing the same directory.

##**Creating a workspace for catkin**
Let's create and build a catkin workspace:
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_make
Additionally, if you look in your current directory you should now have a 'build' and 'devel' folder. Inside the 'devel' folder you can see that there are now several setup.*sh files. Sourcing any of these files will overlay this workspace on top of your environment. To understand more about this see the general catkin documentation: catkin. Before continuing source your new setup.*sh file:
$ source devel/setup.bash
To make sure your workspace is properly overlayed by the setup script, make sure ROS_PACKAGE_PATH environment variable includes the directory you're in.
$ echo $ROS_PACKAGE_PATH
/home/youruser/catkin_ws/src:/opt/ros/kinetic/share
Creating a catkin Package

# You should have created this in the Creating a Workspace Tutorial
$ cd ~/catkin_ws/src
Now use the catkin_create_pkg script to create a new package called 'beginner_tutorials' which depends on std_msgs, roscpp, and rospy:
$ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
This will create a beginner_tutorials folder which contains a package.xml and a CMakeLists.txt, which have been partially filled out with the information you gave catkin_create_pkg.
catkin_create_pkg requires that you give it a package_name and optionally a list of dependencies on which that package depends:
# This is an example, do not try to run this
# catkin_create_pkg <package_name> [depend1] [depend2] [depend3]

##**Building a catkin workspace and sourcing the setup file**
Now you need to build the packages in the catkin workspace:
$ cd ~/catkin_ws
$ catkin_make
After the workspace has been built it has created a similar structure in the devel subfolder as you usually find under /opt/ros/$ROSDISTRO_NAME.
To add the workspace to your ROS environment you need to source the generated setup file:
$ . ~/catkin_ws/devel/setup.bash


##**Understanding ROS Nodes**
Description: This tutorial introduces ROS graph concepts and discusses the use of roscore, rosnode, and rosrun commandline tools.
A node really isn't much more than an executable file within a ROS package. ROS nodes use a ROS client library to communicate with other nodes. Nodes can publish or subscribe to a Topic. Nodes can also provide or use a Service.
ROS client libraries allow nodes written in different programming languages to communicate:
    • rospy = python client library
    • roscpp = c++ client library
roscore is the first thing you should run when using ROS.
rosnode displays information about the ROS nodes that are currently running. The rosnode list command lists these active nodes:
$ rosnode list
    • You will see:
    • /rosout
rosrun allows you to use the package name to directly run a node within a package (without having to know the package path).
$ rosrun turtlesim turtlesim_node
What was covered:
    • roscore = ros+core : master (provides name service for ROS) + rosout (stdout/stderr) + parameter server (parameter server will be introduced later)
    • rosnode = ros+node : ROS tool to get information about a node.
    • rosrun = ros+run : runs a node from a given package.
    
    
##**Understanding ROS Topics**
Description: This tutorial introduces ROS topics as well as using the rostopic and rqt_plot commandline tools.
The turtlesim_node and the turtle_teleop_key node are communicating with each other over a ROS Topic. turtle_teleop_key is publishing the key strokes on a topic, while turtlesim subscribes to the same topic to receive the key strokes.
rqt_graph creates a dynamic graph of what's going on in the system. rqt_graph is part of the rqt package.
The rostopic tool allows you to get information about ROS topics.
