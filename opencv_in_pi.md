
From https://raspberrypi.stackexchange.com/questions/69169/how-to-install-opencv-on-raspberry-pi-3-in-raspbian-jessie
Technico.top

## Install Opencv in Raspbian ##
#### generic stuff ####
```
sudo apt-get update
sudo apt-get upgrade
sudo rpi-update
sudo reboot
sudo apt-get install build-essential git cmake pkg-config
sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install libgtk2.0-dev
sudo apt-get install libatlas-base-dev gfortran
cd ~
git clone https://github.com/Itseez/opencv.git
cd opencv
git checkout 3.1.0
cd ~
git clone https://github.com/Itseez/opencv_contrib.git
cd opencv_contrib
git checkout 3.1.0
```
#### Opencv with Python 2.7 ####
```
sudo apt-get install python2.7-dev
wget https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
pip install numpy
cd ~/opencv
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=OFF \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
    -D BUILD_EXAMPLES=ON ..
make -j4
sudo make install
sudo ldconfig
```
#### Opencv with Python 3 ####
```
sudo apt-get install python3-dev
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
pip install numpy
cd ~/opencv
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=OFF \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
    -D BUILD_EXAMPLES=ON ..
make -j4
sudo make install
sudo ldconfig
```
#### Bonus tips ####

I'd rather use make instead of make -j4. It's 4 times slower, but avoid some compilation errors (course scenario). You still can try make -j4. In cases of errors just use make clean to remove previously compiled stuff, then run make.

I had some difficulties when downloading opencv and opencv-contrib. Instead of cloning the git, you can download the source in tar.gz format here : https://github.com/opencv/opencv/releases
