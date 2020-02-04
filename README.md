# Kimera-VIO-ROS

ROS Wrapper for [Kimera](https://github.com/MIT-SPARK/Kimera).

<div align="center">
    <img src="docs/media/Kimera-VIO-ROS_mesh.gif">
</div>

## Publications

We kindly ask to cite our paper if you find this library useful:

 - A. Rosinol, M. Abate, Y. Chang, L. Carlone. [**Kimera: an Open-Source Library for Real-Time Metric-Semantic Localization and Mapping**](https://arxiv.org/abs/1910.02490). arXiv preprint [arXiv:1910.02490](https://arxiv.org/abs/1910.02490).
 ```bibtex
 @misc{Rosinol19arxiv-Kimera,
   title = {Kimera: an Open-Source Library for Real-Time Metric-Semantic Localization and Mapping},
   author = {Rosinol, Antoni and Abate, Marcus and Chang, Yun and Carlone, Luca},
   year = {2019},
   eprint = {1910.02490},
   archiveprefix = {arXiv},
   primaryclass = {cs.RO},
   url = {https://github.com/MIT-SPARK/Kimera},
   pdf = {https://arxiv.org/pdf/1910.02490.pdf}
 }
```

# 1. Installation

On Ubuntu 16.04 with ROS Kinetic.

```bash
# System dependencies
sudo apt-get update
sudo apt-get install -y --no-install-recommends apt-utils
sudo apt-get install -y \
      cmake build-essential unzip pkg-config autoconf \
      libboost-all-dev \
      libjpeg-dev libpng-dev libtiff-dev \
      libvtk5-dev libgtk2.0-dev \
      libatlas-base-dev gfortran \
      libparmetis-dev \
      python-wstool python-catkin-tools \
      libtbb-dev \
      ros-kinetic-image-geometry \
      ros-kinetic-pcl-ros \
      ros-kinetic-cv-bridge

# Prepare catkin workspace
mkdir -p ~/kimera_workspace/src
cd ~/kimera_workspace
catkin init
catkin config --cmake-args -DCMAKE_BUILD_TYPE=Release -DGTSAM_USE_SYSTEM_EIGEN=ON
catkin config --merge-devel

cd ~/kimera_workspace/src
git clone https://github.com/m-pilia/Kimera-VIO-ROS.git
git --git-dir=${HOME}/kimera_workspace/src/Kimera-VIO-ROS/.git --work-tree=${HOME}/kimera_workspace/src/Kimera-VIO-ROS checkout mynt-eye-s
wstool init
wstool merge Kimera-VIO-ROS/install/kimera_vio_ros_https.rosinstall
wstool update

# Fix build issue on Ubuntu 16.04, see https://github.com/ToniRV/kimera_rviz_markers/issues/1
sed -i '3iset(CMAKE_CXX_STANDARD 11)' ~/kimera_workspace/src/pose_graph_tools/CMakeLists.txt ~/kimera_workspace/src/kimera_rviz_markers/CMakeLists.txt

# Build
catkin build

# 
source ~/kimera_workspace/devel/setup.bash

# To enable the workspace in all bash sessions:
echo 'source ~/kimera_workspace/devel/setup.bash' > ~/.bashrc
```

# 2. Usage

Launch the MYNT EYE ROS wrapper and the Kimera-VIO ROS wrapper (in parallel, e.g. from two separate shells).
```
roslaunch mynt_eye_ros_wrapper display.launch
roslaunch kimera_vio_ros kimera_ros_mymynt.launch
```

# BSD License
KimeraVIO ROS wrapper is open source under the BSD license, see the [LICENSE.BSD](./LICENSE.BSD) file.
