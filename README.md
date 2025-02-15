# ORB-SLAM2 for Formula Student driverless

### Install Eigen3
Required by g2o (see below). Download and install instructions can be found at: http://eigen.tuxfamily.org. Required at least 3.1.0.

### Install Pangolin
Follow the instructions in the following link: https://github.com/stevenlovegrove/Pangolin

### Install OpenCV
#### Install minimal prerequisites (Ubuntu 18.04 as reference)
sudo apt update && sudo apt install -y cmake g++ wget unzip
#### Download and unpack sources
wget -O opencv.zip https://github.com/opencv/opencv/archive/master.zip
unzip opencv.zip
#### Create build directory
mkdir -p build && cd build
#### Configure
cmake  ../opencv-master
#### Build
cmake --build .
#### Install
sudo make install

### Install ORB-SLAM  
git clone https://github.com/fsisel/ORB_SLAM2.git  
cd ORB_SLAM2  
chmod +x build.sh  
./build.sh  

### Test the original ORB-SLAM 2 without ROS 

#### Download datasets and calibration files
cd ~/formula_student_ws/src  
mkdir -p datasets  
cd datasets  
git clone https://github.com/fsisel/calibration  
mkdir -p visual_slam  
cd visual_slam  
wget http://robotics.ethz.ch/~asl-datasets/ijrr_euroc_mav_dataset/machine_hall/MH_01_easy/MH_01_easy.zip  
unzip MH_01_easy.zip  
rm -rf MH_01_easy.zip  

#### Run ORB_SLAM2
cd ~/ORB_SLAM2  
#### Run the following (one single command)  
./Examples/Stereo/stereo_euroc Vocabulary/ORBvoc.txt Examples/Stereo/EuRoC.yaml ~/formula_student_ws/src/datasets/visual_slam/mav0/cam0/data ~/formula_student_ws/src/datasets/visual_slam/mav0/cam1/data Examples/Stereo/EuRoC_TimeStamps/MH01.txt  

### Test ORB-SLAM 2 with the race car
export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:~/ORB_SLAM2/Examples/ROS  
chmod +x build_ros.sh  
./build_ros.sh  

#### Download race car ROS bag
download very_easy_track.bag from IFS Cloud -> Grupos Técnicos -> Grupo VII - Driverless -> Datasets  
put the ROS bag in ~/formula_student_ws/src/datasets/visual_slam

#### Open 3 tabs on the terminal and run the following command at each tab: 
#### Terminal 1  
source /opt/ros/noetic/setup.bash  
roscore  

#### Terminal 2  
source /opt/ros/noetic/setup.bash  
export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:~/ORB_SLAM2/Examples/ROS  
cd ~/ORB_SLAM2
##### Run the following (one single command)
rosrun ORB_SLAM2 Stereo Vocabulary/ORBvoc.txt ~/formula_student_ws/src/datasets/calibration/camera_calibration.yaml false  

#### Terminal 3  
source /opt/ros/noetic/setup.bash
##### Run the following (one single command)
rosbag play --pause ~/formula/student_ws/src/datasets/visual_slam/very_easy_track.bag  /fsds/camera/cam1:=/camera/left/image_raw  /fsds/camera/cam2:=/camera/right/image_raw  

When two windows pop-up you may press 'SPACE' on Terminal 3 to play the bag.
