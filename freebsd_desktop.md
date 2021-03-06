## 第一部分

FreeBSD 12.1-release 安装完成后，默认界面是黑黢黢的终端。我们试着安装 Mate 桌面。

第一步，改源。

先用root账号登录系统。FreeBSD 12.1 默认安装了 vi 和 ee 两个文本阅读器，选择一个适合的

`vi /etc/pkg/FreeBSD.conf`

将文本内 enable 后的 yes 改为 no。这一步是禁止官方源。

然后新建源文本:

`mkdir -p /usr/local/etc/pkg/repos/ #新建文件夹`

`vi /usr/local/etc/pkg/repos/FreeBSD.conf`， 这一步是新建配置文件 FreeBSD.conf

打开后，添加以下内容：
```
FreeBSD: {
  url: "pkg+http://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/quarterly",
 }
```

保存文本后，就可以开始更新源了
```
pkg update
pkg upgrade
```
安装必要软件： sudo 和 vim

`pkg install sudo vim`

vi打开sudo:

`visudo`

在 root ALL=(ALL) ALL 下，添加一行 用户名 ALL=(ALL) ALL,比如你的用户名为 xiaowang, 这一行就是 xiaowang ALL=(ALL) ALL 保存后退出。

开始安装 Mate 桌面：

`pkg install mate mate-desktop xorg`

这一步大概要安装近三百个软件，需要耗费大量时间，你可以去忙别的事情了。不过要检查一下是否有超时现象，到时候再执行一遍本命令，继续下载即可。

安装成功后，继续配置

`vim /etc/rc.conf`

添加以下内容：
```
moused_enable="YES"
dbus_enable="YES"
hald_enable="YES"
```
安装显示管理器（登录管理器）：

`pkg install slim`

启用登录管理器:

`vim /etc/rc.conf`， 添加一行 `slim_enable="YES"`

默认用户自动登录：

`vim /usr/local/etc/slim.conf` ，找到 auto_login,取消注释，这一行变为auto_login yes。同时找到 default_user , 这一行变为default_user 你的用户名 。保存后退出。

退出系统，

 `exit`

使用普通账号登录系统。

 `vim .xinitrc` (创建一个新文档)

添加一行 `exec mate-session`

至此，桌面系统安装完成。重启系统，即可看到 slim 界面，输入账号密码进入系统。


## 第二部分

### 显卡驱动

`sudo pkg install drm-kmod`

drm-kmod 为从 linux 移植的 intel/amd 显卡驱动,安装完成后需要手动添加。打开 `/etc/rc.conf` ，如下:

- 如果为 intel 核心显卡，添加 kld_list="/boot/modules/i915kms.ko"。

- 如果为 HD7000 以后的 AMD 显卡，添加kld_list="/boot/modules/amdgpu.ko"。

- 如果为 HD7000 以前的 AMD 显卡，添加kld_list="/boot/modules/radeonkms.ko"。

### 视频硬解

`sudo pkg install xf86-video-intel libva-intel-driver`

### 一些软件

- 中文字体： `pkg install noto-sc`

- 网络管理器： `pkg install networkmgr`

- 软件包管理器：`octopkg`

- 图标：`papirus-icon-theme`

### 中文输入法： 

`pkg install zh-fcitx zh-fcitx-libpinyin zh-fcitx-configtool`

根据安装后的提示，执行 vim .xinitrc，添加以下内容
```
export XMODIFIERS='@im=fcitx'
export GTK_IM_MODULE=fcitx
export GTK3_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
```
### 输入法配置

安装好 fcitx 后，使用 Configure来配置，可以保留汉语和英语两种输入法，如 pinyin(LibPinYin)和Keyboard - English(US)。默认 Ctrl + 空格切换。

### 无线模块

查看无线硬件型号： `> pciconf -lv`

以我的笔记本为例，无线型号为 RTL8188CU 。查阅手册发现，开启 rtwn 模块即可开启无线，步骤如下（其它无线模块大同小异）：

打开 `/boot/loader.conf `,添加以下内容：
```
if_rtwn_load="YES"

if_rtwn_pci_load="YES"

legal.realtek.license_ack=1
```
打开 `/etc/rc.conf`，添加以下内容

```
wlans_rtwn0="wlan0"
ifconfig_wlan0="WPA DHCP" 
```

`vim /etc/wpa_suppplicant.conf`，添加：

```
network={
    ssid="无线名称"
    psk="无线密码"
    }
```
然后重启` netif`

`sudo /etc/rc.d/netif restart`

如何仍不起作用，终端输入：

`sudo ifconfig wlan0 up`

### 修改 sh

chsh 用户名，然后编辑文件即可, 如 sh 一栏改为 /usr/csh or /usr/local/bin/bash。建议默认设置为 csh 。
终端(csh)配置输入法

在 .cshrc 和 /etc/csh.cshrc 中添加如下：

```
setenv XMODIFIERS @im=fcitx
setenv GTK_IM_MODULE fcitx
setenv GTK2_IM_MODULE fcitx
setenv GTK3_IM_MODULE fcitx
setenv QT_IM_MODULE fcitx
setenv QT4_IM_MODULE fcitx
```

### 设置终端识别和输入中文

在 .cshrc 和 /etc/csh.cshrc 中添加两行：

```
setenv LANG en_US.UTF-8
setenv MM_CHARSET en_US.UTF-8
```
### 自动挂载U盘

`pkg install automount`

### 对应软件：

fusefs-ext4fuse : 只读

fusefs-ext2 : 支持读写 ext2, ext3, ext4

fusfefs-lkl : 将 linux 内核变为库文件，支持读写 BTFRS, XFS, EXT3, EXT4

## 第三部分

### 使用技巧

- 显示硬件： `vim /var/run/dmesg.boot`

- 登录信息： `vim /var/log/messages`

### 部分软件

vscode linux-fdisk ddrescue redshift plank unrar you-get( pip版本较新) linux-sublime3 linuxqq

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

`sudo pkg install linux_base-c7`

打开 `/etc/rc.conf`，添加：

`linux_enable="YES"`

### Linux for QQ

注：QQ 为 2019年10月在 FreeBSD 12 上的测试，后续版本未作进一步测试。

先安装两个依赖库文件：

`sudo pkg install linux-c7-gtk2 linux-c7-nss`

后续步骤：

- 1.去腾讯官网下载 x64 架构的 sh 文件

- 2.终端运行此 sh 文件，得到一个压缩包

- 3.将压缩包解压，运行 qq 二进制文件

提示：需要手机 QQ 扫码登录

### Reaper for Linux

下载 reaperXXX_linux_x86_64,解压后，按照指示步骤安装。终端启动，会显示如下错误

`./reaper: error while loading shared libraries: libasound.so.2: cannot open shared object file: No such file or directory`

可以看出，是缺乏库文件所致，解决方法为：

`sudo pkg install linux-c7-alsa-lib alsa-lib`

然后就可成功开启，测试 reaper 6.0.8 成功运行

### WPS for Linux
