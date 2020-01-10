## AMD (未经测试)

### 公有驱动 [参考文档](https://wiki.debian.org/AtiHowTo)

### 私有驱动 [参考文档](https://wiki.debian.org/AMDGPUDriverOnStretchAndBuster2)

1 访问[AMD官方驱动网址](https://www.amd.com/en/support/kb/release-notes/rn-amdgpu-unified-linux)，
  点击下载*Radeon™ Software for Linux® version 19.X0 for Ubuntu 1X.XX.X*，这是一个后缀为 tar.gz 的
  压缩文档。
  
2 安装内核头文件，`sudo apt install linux-headers-<version>-amd64`,内核版本可以用`uname -r`查看。

3 解压第一步下载好的压缩文档： `tar -xvf amdgpu-pro-19.X0-abcdef-ubuntu-1X.XX.tar.xz`。

4 安装解压后的两个安装包：`dpkg -i amdgpu-core_1X.x0-abcdef_all.deb amdgpu-dkms_1X.X0-abcdef_all.deb`

5 重启系统

6 查看驱动：`dmesg | grep "amdgpu version"`
