# Ubuntu16.04 cuda 8.0

## 更新源

    cd /etc/apt/
    sudo cp sources.list sources.list.bak
    sudo gedit sources.list
在sources.list文件头部添加如下源：

    deb http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
    deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
    deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
    deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
    deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
    deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
    deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
    deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
    deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
    deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
然后更新源和安装的包:

    sudo apt-get update

    sudo apt-get upgrade
    
## 安装NVIDIA显卡驱动

采用ppa安装方式，没选择最新的nvidia-370，我选择了nvidia-367。

Ctrl+Alt+F1进入tty命令控制台，停止lightdm，然后开始安装驱动。

    $ sudo service lightdm stop

    sudo add-apt-repository ppa:xorg-edgers/ppa
    sudo apt-get updates
    sudo apt-get install nvidia-367
    sudo apt-get install mesa-common-dev
    sudo apt-get install freeglut3-dev
    sudo reboot
将显示器VGA接口换到NVIDIA显卡上。

## 修改分辨率

启动到界面之后发现分辨率只有1366x768，显示器适合1920x1080，采用xrandr并修改xorg.conf来解决。

    sudo gedit /etc/X11/xorg.conf
修改如下：

    HorizSync 31.0 - 84.0
    VertRefresh 56.0-77.0
即最终的xorg.conf文件为：

    Section "Device"    
        Identifier "Configured Video Device"
    EndSection

    Section "Monitor"
        Identifier "Configured Monitor"
        Horizsync 30-84
        Vertrefresh 56-77
    EndSection

    Section "Screen"
    Identifier "Default Screen"
    Monitor "Configured Monitor"
    Device "Configured Video Device"
        SubSection "Display"
            Modes "1920x1080" "1360x768" "1024x768" "1152x864"
        EndSubSection
    EndSection        
注销系统再次登录后，选择适合的桌面分辨率即可。

## 安装CUDA8.0

到官网下载cuda_8.0.44_linux.run，复制到根目录下。

    sudo sh cuda_8.0.44_linux.run --tmpdir=/tmp/
遇到问题：incomplete installation，然后执行

    sudo apt-get install freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libgl1-mesa-glx libglu1-mesa libglu1-mesa-dev
    sudo sh cuda_8.0.44_linux.run -silent -driver
注：此时安装过程中提示是否要安装NVIDIA驱动时选择no。其他选择yes或默认即可。

    Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 361.62? (y)es/(n)o/(q)uit: n
安装完毕后声明环境变量：

    sudo gedit ~/.bashrc
在.bashrc尾部添加如下内容：

    export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}