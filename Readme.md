# ASUS ZENBOOK PRO DUO

## window

기본 설정
```
비밀번호 usrgasus
서비스 지원 운영체제 선택 후 (window)
```
깔면 안되는 것
```
wireless 버전 확인 (맨 위)
ISST (소리)
BIOS는 이미 깔려있음
```

깔 것
```
Utility
ASUS System Control Interface V2(Driver) 버전 확인
ASUS Alexa Extension
ScreenPad Plus Optimizer V1.0.0.3 깔고 마지막에 실행 (최적화)
Thunderbolt Control Center

NVIDIA Control Panel
Intel Graphic Control Panel (그래픽 카드는 해당 회사 사이트에서 다운 받지 않는 이상 깔아야함)

ScreenXpert (Windows Store App)
MyASUS
```
ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ

## Ubuntu 18.04

### wifi
*하는 방법
```
https://www.ostechnix.com/different-ways-to-update-linux-kernel-for-ubuntu/
```
*커널 메뉴얼 다운로드
```
https://kernel.ubuntu.com/~kernel-ppa/mainline/
```
*재부팅후 커널 오류나면 bios 에서 security boot 해제

*firmware 다운로드
```
https://www.intel.com/content/www/us/en/support/articles/000005511/network-and-io/wireless-networking.html
```

### nvidia
*현재 깔린 nvidia 확인
```
dpkg -l | grep nvidia 
```
apt 로 깔수있는 nvidia 정리 (Ubuntu 18.04.3 에서만 418로 깔수있다. / Ubuntu 18.04.4 에서는 깔때 쉽지 않음...)
```
apt-cache search nvidia
```

설치
```
sudo apt-get install nvidia-driver-418 (아마 자동으로 430으로 깔릴것)
sudo reboot
```

### CUDA-10-0
nvidia 에서 cuda-10-0 ubuntu deb 파일 다운로드 이후
```
sudo dpkg -i cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64.deb
sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda

혹시 마지막 줄이 runtime / drivers 문제로 안된다면 그 두개 --purge 로 강제 다운한 뒤 sudo apt-get install cuda-toolkit-10-0 으로 다운
```
*verify
```
cat /usr/local/cuda/version.txt 로 확인
```

### CUDNN
nvidia 에서 cudnn-7.5.0 cuda-10.0 for linux 로 다운로드
```
sudo mv cudnn-10.0-~~ /usr/local/cuda-10.0 옮기고 옮긴 폴더로 이동
sudo tar -zxvf cudnn-10.0-~~
```
*복사
```
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda-10.0/lib64/
sudo cp cuda/include/cudnn.h /usr/local/cuda-9.0/include/
```
*권한 부여
```
sudo chmod a+r /usr/local/cuda-10.0/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```
*verify
```
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2 로 확인
```

### Tensorflow (Python 2.7)
```
sudo apt-get install python-pip
pip install --user tensorflow-gpu==1.14
```

*verify
```
import tensorflow as tf
tf.__version__
```

### ROS
```
sudo wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_melodic.sh && sudo chmod 755 ./install_ros_melodic.sh && sudo bash ./install_ros_melodic.sh
```

### Q_Groundcontrol
*링크 타고 확인
```
https://docs.qgroundcontrol.com/en/getting_started/download_and_install.html (gstreamer1.0  깔때 px4 firmware 깔때처럼 종류 다 깔면 좋음)
```

### px4 firmware
*MAVROS 설치
```
sudo apt install ros-melodic-mavros ros-melodic-mavros-extras
```

*PX4 Firmware 설치
```
git clone https://github.com/PX4/Firmware.git
cd Firmware
git submodule update --init --recursive
```

*pip3 설치 (Firmware 설치시 pip3 필요)
```
sudo apt-get install python3-pip
```

*pip setting
```
sudo -H pip3 install --upgrade pip
sudo -H pip2 install --upgrade pip
```

*이거 필요한가? (일단 안 깔아도 될듯)
```
sudo apt-get install libignition-math2-dev //? need?
```

*깔아라고 나올거임
```
pip3 install --user toml
pip3 install --user numpy
pip3 install --user empy
pip3 install --user junja2
```

*gstreamer1.0 error 나면 아래 설치
```
sudo apt install libgstreamer1.0-dev
sudo apt install gstreamer1.0-plugins-good
sudo apt install gstreamer1.0-plugins-bad
sudo apt install gstreamer1.0-plugins-ugly
```

*add below line in ./bashrc
```
source /home/usrg-asus/Firmware/Tools/setup_gazebo.bash /home/usrg-asus/Firmware /home/usrg-asus/Firmware/build/px4_sitl_default
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:/home/usrg-asus/Firmware
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:/home/usrg-asus/Firmware/Tools/sitl_gazebo
```

*ERROR HANDLING
```
[FATAL] [1581000160.506215586]: UAS: GeographicLib exception: File not readable /usr/share/GeographicLib/geoids/egm96-5.pgm | Run install_geographiclib_dataset.sh script in order to install Geoid Model dataset!
--> sudo geographiclib-get-geoids egm96-5
```

### etc permission
*ros 실행하는데 permission error 나오면 아래 실행
```
sudo rosdep fix-permissions
```
