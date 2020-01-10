## AMD (未经测试)

### 公有驱动 [参考文档](https://wiki.debian.org/AtiHowTo)

- 查看一下显卡型号： `lspci -nn | grep VGA`

 

- 删除（可能）已有的英伟达驱动，注意点号：`apt purge nvidia.`。

- 打开文档 `/etc/apt/sources.list`，添加 contrib 和 non-free 源，如：

  >  deb http://mirrors.ustc.edu.cn/debian buster main contrib non-free
  
- 更新源文件：`sudo apt update`

- 安装驱动：
 
  较新显卡： > apt install firmware-linux-nonfree libgl1-mesa-dri xserver-xorg-video-amdgpu 
 
  或者
 
  较旧显卡： > apt install firmware-linux-nonfree libgl1-mesa-dri xserver-xorg-video-ati
  
- 重启系统
  

### 私有驱动 [参考文档](https://wiki.debian.org/AMDGPUDriverOnStretchAndBuster2)

1. 访问[AMD官方驱动网址](https://www.amd.com/en/support/kb/release-notes/rn-amdgpu-unified-linux)，
  点击下载*Radeon™ Software for Linux® version 19.X0 for Ubuntu 1X.XX.X*，这是一个后缀为 tar.gz 的
  压缩文档。
  
2. 安装内核头文件，`sudo apt install linux-headers-<version>-amd64`,内核版本可以用`uname -r`查看。

3. 解压第一步下载好的压缩文档： `tar -xvf amdgpu-pro-19.X0-abcdef-ubuntu-1X.XX.tar.xz`。

4. 安装解压后的两个安装包：`dpkg -i amdgpu-core_1X.x0-abcdef_all.deb amdgpu-dkms_1X.X0-abcdef_all.deb`

5. 重启系统

6. 查看驱动：`dmesg | grep "amdgpu version"`



## Intel

开箱即用，无需安装。可以查看一下软件包 `xserver-xorg-core`，应该已默认安装。


## Nvidia [参考文档](https://wiki.debian.org/NvidiaGraphicsDrivers#Installation)

- 前面几步和安装amd驱动类似，增加闭源源文件，安装内核头文件。

- 安装驱动：

 >  sudo apt install nvidia-driver 




