# ORB-SLAM2 for Formula Student driverless

## Install Eigen3
Required by g2o (see below). Download and install instructions can be found at: http://eigen.tuxfamily.org. Required at least 3.1.0.

## Install Pangolin
Follow the instructions in the following link: https://github.com/stevenlovegrove/Pangolin

## Installl OpenCV
### Install minimal prerequisites (Ubuntu 18.04 as reference)
sudo apt update && sudo apt install -y cmake g++ wget unzip
### Download and unpack sources
wget -O opencv.zip https://github.com/opencv/opencv/archive/master.zip
unzip opencv.zip
### Create build directory
mkdir -p build && cd build
### Configure
cmake  ../opencv-master
### Build
cmake --build .
### Install
sudo make install

## ORB-SLAM
git clone https://github.com/Windfisch/ORB_SLAM2.git
cd ORB_SLAM2
chmod +x build.sh
./build.sh

## Test without ROS
cd ~/formula_student_ws/src
mkdir -p datasets
cd datasets
wget http://robotics.ethz.ch/~asl-datasets/ijrr_euroc_mav_dataset/machine_hall/MH_01_easy/MH_01_easy.zi
cd ~/ORB_SLAM2
./Examples/Stereo/stereo_euroc Vocabulary/ORBvoc.txt Examples/Stereo/EuRoC.yaml ~/formula_student_ws/src/datasets/mav0/cam0/data ~/formula_student_ws/src/datasets/mav0/cam1/data Examples/Stereo/EuRoC_TimeStamps/MH01.txt

## Test with ROS
export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:~/ORB_SLAM2/Examples/ROS
chmod +x build_ros.sh
./build_ros.sh
cd ~/formula_student_ws/src/datasets
wget http://robotics.ethz.ch/~asl-datasets/ijrr_euroc_mav_dataset/vicon_room1/V1_01_easy/V1_01_easy.bag
Follow the instructions here: https://github.com/Windfisch/ORB_SLAM2#running-stereo-node

## Test with a ROS Bag record on the Formula Student Driverless Simulator

