## 新手指导

本教程旨在指导 debian 的无线安装，若是初次尝试 Linux 的爱好者，建议直接下载[带驱动的的镜像版](https://mirrors.ustc.edu.cn/debian-cdimage/unofficial/non-free/cd-including-firmware/10.4.0-live%2Bnonfree/amd64/iso-hybrid/)。

## 修改源

打开 `/apt/apt/sources.list`，修改源地址为：

> deb http://mirrors.ustc.edu.cn/debian buster main contrib non-free

或者

> deb http://mirrors.ustc.edu.cn/debian bullseye main contrib non-free


以上分别为 debian 10 和 debian 11 源文件，二者选其一。

## 终端安装

### 有线网络

更新源文件后，可在有线条件下，终端里直接安装，查找列表里显示的对应驱动的安装包即可。

### 无网条件下

手机（或其它支持网络的设备）浏览器打开[debian 10 固件驱动包](https://mirrors.ustc.edu.cn/debian-cdimage/unofficial/non-free/firmware/buster/current/firmware.zip) 或 [debian 11 固件驱动包](https://mirrors.ustc.edu.cn/debian-cdimage/unofficial/non-free/firmware/bullseye/current/firmware.zip) 。

将压缩包放入电脑中，解压找到需要的无线驱动，安装即可。

## 无线列表

- firmware-ath9k-htc ： 支持 ar9271 和 ar7010 。

- firmware-realtek :  RTLxxxx 系列无线 。
