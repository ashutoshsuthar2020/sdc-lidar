Found no problem in my installation.

# Install and setup the Intel RealSense D435 camera

First, install librealsense2, following these instructions: https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md

1.  Register the server's public key:
```
sudo apt-key adv --keyserver keys.gnupg.net --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE
```
2. Add the server to the list of repositories (only for Ubuntu 20 LTS):
```
sudo add-apt-repository "deb https://librealsense.intel.com/Debian/apt-repo focal main" -u
```
3. Install the libraries (see section below if upgrading packages):
```
sudo apt-get install librealsense2-dkms
sudo apt-get install librealsense2-utils
```

##### The above two lines will deploy librealsense2 udev rules, build and activate kernel modules, runtime library and executable demos and tools.


4. Reconnect the Intel RealSense depth camera and run: realsense-viewer to verify the installation.


# Installing the ROS Wrapper for Intel RealSense Devices

Following the instructions from https://github.com/IntelRealSense/realsense-ros
```
sudo apt install ros-noetic-realsense2-camera
sudo apt install ros-noetic-realsense2-description
```

To test the installation, run:
```
roslaunch realsense2_camera rs_d435_camera_with_model.launch
```

If you find a problem like this (like I did), just close the terminal and open a new one to refresh the source directories:
```
Failed to load nodelet [/camera/realsense2_camera] of type [realsense2_camera/RealSenseNodeFactory] even after refreshing the cache: 
Failed to load library /opt/ros/noetic/lib//librealsense2_camera.so. 
Make sure that you are calling the PLUGINLIB_EXPORT_CLASS macro in the library code, and that names are consistent between this macro and your XML. 
Error string: Could not load library (Poco exception = librealsense2.so.2.44: cannot open shared object file: No such file or directory)
```
