# Orbbec Astra on ROS Kinetic

install libraries:
	sudo apt install ros-kinetic-rgbd-launch ros-kinetic-libuvc ros-kinetic-libuvc-camera ros-kinetic-libuvc-ros ros-kinetic-astra-camera ros-kinetic-astra-launch ros-kinetic-image-view
	sudo apt-get install --reinstall ros-kinetic-openni-camera ros-kinetic-openni-launch

(based on the guide in https://github.com/orbbec/ros_astra_camera)

create a workspace:
	mkdir -p ~/astra_ws/src
	cd ~/astra_ws
	source /opt/ros/kinetic/setup.bash
	catkin_make

source devel/setup.bash

	cd ~/astra_ws/src
	git clone https://github.com/orbbec/ros_astra_camera.git
	cd ..
	catkin_make

create a file:
    sudo nano /etc/udev/rules.d/orbbec-usb.rules

copy and paste and save:

SUBSYSTEM=="usb", ATTR{idProduct}=="0401", ATTR{idVendor}=="2bc5", MODE:="0666", OWNER:="root", GROUP:="video"
SUBSYSTEM=="usb", ATTR{idProduct}=="0402", ATTR{idVendor}=="2bc5", MODE:="0666", OWNER:="root", GROUP:="video"
SUBSYSTEM=="usb", ATTR{idProduct}=="0403", ATTR{idVendor}=="2bc5", MODE:="0666", OWNER:="root", GROUP:="video"
SUBSYSTEM=="usb", ATTR{idProduct}=="0404", ATTR{idVendor}=="2bc5", MODE:="0666", OWNER:="root", GROUP:="video"
SUBSYSTEM=="usb", ATTR{idProduct}=="0405", ATTR{idVendor}=="2bc5", MODE:="0666", OWNER:="root", GROUP:="video"

reload udev rules:
    sudo udevadm control --reload-rules

important!!!! unplug and plug back the camera

check if the camera is installed:
	lsusb | grep 2bc5

Should give something like:
	Bus 001 Device 095: ID 2bc5:0401

launch camera:
	roslaunch astra_launch astra.launch

view camera:
	rosrun image_view image_view image:=/camera/rgb/image_raw
	rosrun image_view image_view image:=/camera/depth/image_raw
	rosrun image_view image_view image:=/camera/depth/image_rect_raw
	rosrun image_view image_view image:=/camera/ir/image

list cameras:
	rosrun astra_camera astra_list_device

Use Astra Stereo S (w/ UVC)

	roslaunch astra_camera stereo_s.launch

nice guide on calibration and rtab_map:
https://scott-yantek.squarespace.com/blog/2018/1/16/rtab-map-setup-with-orbbec-astra-in-ros
