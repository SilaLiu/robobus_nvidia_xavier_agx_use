<img width="813" alt="image" src="https://github.com/SilaLiu/robobus_nvidia_xavier_agx_use/assets/39790272/272cb628-56a4-434b-9759-cca75358f04e">



### Configure the /etc/hosts file
#### IPC:
    sudo vim /etc/hosts

    192.168.4.103	    nvidia-master
    192.168.44.101	    nvidia-desktop

#### nvidia-master:
    sudo vim /etc/hosts

    192.168.4.103	    nvidia-master
    192.168.4.88		t-Default-string

#### nvidia-desktop:
    sudo vim /etc/hosts

    192.168.44.101	    nvidia-desktop
    192.168.4.88		t-Default-string




### Configure the bashrc file

#### IPC-IP：192.168.4.88
    sudo vim ~/.bashrc

    export ROS_HOSTNAME=t-Default-string                               # ros master ip 
    export ROS_MASTER_URI=http://t-Default-string:11311                # ros master URI


#### nvidia-master-IP:192.168.4.103
    sudo vim ~/.bashrc

    export ROS_HOSTNAME=nvidia-master                                  # ros slave 1
    export ROS_MASTER_URI=http://t-Default-string:11311                # ros master URI


#### nvidia-desktop-IP:192.168.44.101
    sudo vim ~/.bashrc
    export ROS_HOSTNAME=nvidia-desktop                                 # ros slave 2
    export ROS_MASTER_URI=http://t-Default-string:11311                # ros master URI


### run 
    IPC:$roscore
    
    nvidia-master:$rostopic list
    
    nvidia-desktop:$rostopic list
