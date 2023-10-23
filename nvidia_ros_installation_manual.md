
### Installation Ros
---
##### Setup your sources.list
##### Setup your computer to accept software from packages.ros.org.  
    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'


#### Set up your keys
    sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

    
#### Installation
    sudo apt update
  
  Now pick how much of ROS you would like to install.
##### Desktop-Full Install: (Recommended) : Everything in Desktop plus 2D/3D simulators and 2D/3D perception packages
    sudo apt install ros-melodic-desktop-full


### Initialise rosdep
    sudo rosdep init
    rosdep update

### Setting up the environment
    echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
    source ~/.bashrc

### Building Factory Dependencies
    sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential

### Completing the Installation
  Run ros to test if the installation was successful
    
    roscore
