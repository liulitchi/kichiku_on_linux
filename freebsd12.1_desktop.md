## 第一部分
    
安装 FreeBSD 12.1 后，默认界面是黑黢黢的终端。试着安装一下 Mate 桌面，并进行一些后续操作，做一下记录，以供查阅。
    
第一步，改源。
先用root账号登录系统。FreeBSD 12.1 默认安装了 vi 和 ee 两个文本阅读器，选择一个适合的

vi /etc/pkg/FreeBSD.conf

将文本内enable后的yes ，改为 no。这一步是禁止官方源。
    
 然后新建源文本。
 mkdir -p /usr/local/etc/pkg/repos/  这一步是新建文件夹

vi /usr/local/etc/pkg/repos/FreeBSD.conf 这一步是新建配置文件 FreeBSD.conf
  
  
打开后，添加以下内容：
    
```
FreeBSD: {
  url: "pkg+http://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/quarterly",
 }
```
<s>修改operation timed out：</s>

<s>打开 /usr/local/etc/pkg.conf,  查找 fetch_entry 一行，取消注释，并改为 fetch_entry = 10 ;同理 fetch_timeout = 200(或者为 fetch_time)</s>

保存文本后，就可以开始更新源了
    
> pkg update
    
> pkg upgrade
    
    
安装必要软件： sudo 和 vim
    
> pkg install sudo vim
   
   
vi打开sudo:
    
> visudo 
    
在 root ALL=(ALL) ALL 下，添加一行 `用户名 ALL=(ALL) ALL`,比如你的用户名为 xiaowang, 这一行就是 `xiaowang ALL=(ALL) ALL`
保存后退出。
    
开始安装 Mate 桌面：
    
> pkg install mate mate-desktop xorg 
    
这一步大概要安装近三百个软件，需要耗费大量时间，你可以去忙别的事情了。不过要检查一下是否有超时现象，到时候再执行一遍本命令，继续下载即可。
    
安装成功后，继续配置
    
> vim /etc/rc.conf
添加以下内容：
```
moused_enable="YES"
dbus_enable="YES"
hald_enable="YES"
````




安装显示管理器（登录管理器）：
    
> pkg install slim
    
启用登录管理器:
    
> vim /etc/rc.conf 里添加一行 `slim_enable="YES"`

默认用户自动登录：

> vim /usr/local/etc/slim.conf ，找到 auto_login,取消注释， 这一行变为`auto_login yes`。同时找到 default_user , 这一行变为`default_user   你的用户名`  。保存后退出。
    
退出系统，使用普通账号登录系统。
    
> cd ~
    
> vim .xinitrc (创建一个新文档)
    
添加一行 `exec mate-session`
    
至此，桌面系统安装完成。重启系统，即可看到 slim 界面，输入账号密码进入系统。
    
    
## 第二部分 

### [显卡驱动](https://wiki.freebsd.org/Graphics#AMD_Graphics)

> sudo pkg install drm-kmod

这个 drm-kmod 就是移植的 linux 版的 intel/amd 显卡驱动,安装完成之后需要手动添加。打开 /etc/rc.conf ，对应显卡如下:

- 如果为 intel 核心显卡，添加 `kld_list="/boot/modules/i915kms.ko"`。

- 如果为 HD7000 以后的 AMD 显卡，添加`kld_list="/boot/modules/amdgpu.ko`。

- 如果为 HD7000 以前的 AMD 显卡，添加`kld_list="/boot/modules/radeonkms.ko`。

### 视频硬解

> sudo pkg install xf86-video-intel libva-intel-driver
    
添加中文字体： pkg install noto-sc
    
安装网络管理器： pkg install networkmgr

安装软件包管理器：octopkg
    
安装中文输入法： pkg install zh-fcitx zh-fcitx-libpinyin zh-fcitx-configtool
    
根据安装后的提示，执行 vim .xinitrc，添加以下内容
    
```
export XMODIFIERS='@im=fcitx'
export GTK_IM_MODULE=fcitx
export GTK3_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
```

输入法配置

安装好 fcitx 后，使用 `Configure`来配置，可以保留汉语和英语两种输入法，如 `pinyin(LibPinYin)`和`Keyboard - English(US)`。默认 Ctrl + 空格切换。

### 无线模块
    
查看无线硬件型号： > pciconf -lv
    
我的笔记本显示出无线型号为 RTL8188CU，查阅手册发现，开启 rtwn 模块即可开启无线，步骤如下（其它无线模块大同小异）：
    
打开 `/boot/loader.conf` ,添加以下内容：

```   
if_rtwn_pci_load="YES"

legal.realtek.license_ack=1
```
    
打开 `/etc/rc.conf`，添加以下内容

```   
wlans_rtwn0="wlan0"

ifconfig_wlan0="WPA DHCP" 
```

然后重启 netif

> sudo /etc/rc.d/netif restart

如何仍不起作用，终端再输入

> sudo ifconfig wlan0 up


### 修改 sh

chsh 用户名，然后编辑文件即可, 如  sh 一栏改为 /usr/local/bin/bash

### 自动挂载U盘

> pkg install automount
 
## 第三部分

### 使用技巧

- 显示硬件： vim /var/run/dmesg.boot

- 登录信息： vim /var/log/messages


### 部分软件

vscode linux-fdisk ddrescue redshift plank unrar you-get( pip 内版本较新) linux-sublime3 linuxqq

### neofetch --off 输出

```
OS: FreeBSD 12.1-RELEASE amd64 
Uptime: 25 mins 
Packages: 586 (pkg) 
Shell: bash 5.0.16 
Resolution: 1366x768 
DE: MATE 
WM: Metacity (Marco) 
WM Theme: Menta 
Theme: Menta [GTK2/3] 
Icons: Papirus [GTK2/3] 
Terminal: mate-terminal 
Terminal Font: Monospace 10 
CPU: Intel i5-2540M (4) @ 2.594GHz 
GPU: 2nd Generation Core Processor Family Integrated Graphics Controller 
Memory: 2487MiB / 7705MiB 
```


### 测试安装 Linux 兼容软件 

> sudo pkg install linux_base-c7

打开 /etc/rc.conf，添加：

> linux_enable="YES"

#### [Linux for QQ](https://www.bilibili.com/video/BV1LE411a7gw)
 
注：以下为 2019年10月份对 FreeBSD 12 的测试，后续版本未作进一步测试。

先安装两个依赖库文件：
 
 > sudo pkg install linux-c7-gtk2 linux-c7-nss
 
 

后续步骤：

- 1.去腾讯官网下载 x64 架构的 sh 文件

- 2.终端运行此 sh 文件，得到一个压缩包

- 3.将压缩包解压，运行 qq 二进制文件

提示：需要手机 QQ 扫码登录

#### Reaper for Linux

下载 reaperXXX_linux_x86_64,解压后，按照指示步骤安装。终端启动，会显示如下错误

`./reaper: error while loading shared libraries: libasound.so.2: cannot open shared object file: No such file or directory
`
可以看出，是缺乏库文件所致，解决方法为：

> sudo pkg install linux-c7-alsa-lib alsa-lib
    
#### WPS for Linux
