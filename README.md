# OpenFace-Install everything necessary for OpenFace to compile in Unix Server 

#!/bin/bash
#==============================================================================
# Title: install.sh
# Description: Install everything necessary for OpenFace to compile.
# Author: Riyaz  G <griyaz@outlook.com>
# Date: 20170428
# Version : 1.0
# Usage: bash install.sh
# NOTES: There are certain steps to be taken in the system before installing 
#        via this script (refer to README):
# Exit script if any command fails
set -e 
set -o pipefail

if [ $# -ne 0 ]
  then
    echo "Usage: install.sh"
    exit 1
fi

# Essential Dependencies
echo "Installing Essential dependencies..."
sudo yum -y update
sudo yum -y install build-essential
sudo yum -y install llvm
sudo yum -y install clang-3.7 libc++-dev libc++abi-dev
sudo yum -y install cmake
sudo yum -y install libopenblas-dev liblapack-dev
sudo yum -y install git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo yum -y install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev checkinstall
echo "Essential dependencies installed."

# OpenCV Dependency
echo "Downloading OpenCV..."
wget https://github.com/Itseez/opencv/archive/3.1.0.zip
unzip 3.1.0.zip
cd opencv-3.1.0
mkdir -p gitbuild
cd gitbuild
echo "Installing OpenCV..."
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D BUILD_SHARED_LIBS=OFF ..
make -j4
sudo make install
cd ../..
rm 3.1.0.zip
sudo rm -r opencv-3.1.0
echo "OpenCV installed."

# Boost C++ Dependency
echo "Installing Boost..."
sudo yum install libboost-all-dev
echo "Boost installed."

# OpenFace installation
echo "Installing OpenFace..."
mkdir -p gitbuild
cd gitbuild
cmake -D CMAKE_BUILD_TYPE=RELEASE ..
make
cd ..
echo "OpenFace successfully installed."
