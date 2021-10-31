

## 第一部分

FreeBSD 13-release 安装完成后，默认界面是黑黢黢的终端。我们试着安装 MATE 桌面。

- 1 改源:

先用root账号登录系统。FreeBSD 默认安装了 vi 和 ee 两个文本阅读器，选择一个适合的



新建源文本:

`mkdir -p /usr/local/etc/pkg/repos/ #新建文件夹`

`vi /usr/local/etc/pkg/repos/FreeBSD.conf`， 这一步是新建配置文件 FreeBSD.conf

打开后，添加以下内容：
```
ustc:{
　　url: "pkg+http://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/quarterly", 
　　mirror_type: "srv",
　　signature_type: "none",
　　fingerprints: "/usr/share/keys/pkg",
　　enabled: yes
}
```

禁用系统级 pkg 源：

 > mv /etc/pkg/FreeBSD.conf /etc/pkg/FreeBSD.conf.back


保存文本后，就可以开始更新源了
```
pkg update
pkg upgrade
```
安装必要软件： sudo 和 vim

`pkg install sudo vim`

vi打开sudo: `visudo`

在 root ALL=(ALL) ALL 下，添加一行 用户名 ALL=(ALL) ALL,比如你的用户名为 xiaowang, 这一行就是 xiaowang ALL=(ALL) ALL 保存后退出。

安装 MATE桌面和显示管理器（登录管理器）：

`pkg install xorg slim mate`


安装成功后，继续配置

`vim /etc/rc.conf`

添加以下内容：
```
moused_enable="YES"
dbus_enable="YES"
hald_enable="YES"

slim_enable="YES"
```

自动登录图形管理器：

`vim /usr/local/etc/slim.conf` ，找到 auto_login,取消注释，这一行变为auto_login yes。同时找到 default_user , 这一行变为default_user 你的用户名 。保存后退出。

退出系统，`exit`

至此，设置接近完毕，最后一步，使用普通账号登录系统。

 `vim .xinitrc` (创建一个新文档)

添加一行 `exec mate-session`


## 第二部分

### 显卡驱动

`sudo pkg install drm-fbsd13-kmod`

drm-xxx-kmod 为从 linux 移植的 intel/amd 显卡驱动,安装完成后需要手动添加。打开 `/etc/rc.conf` ，如下:

- 如果为 intel 核心显卡，添加 kld_list="i915kms"。

- 如果为 HD7000 以后的 AMD 显卡，添加kld_list="amdgpu"。

- 如果为 HD7000 以前的 AMD 显卡，添加kld_list="radeonkms"。

### 视频硬解

`sudo pkg install xf86-video-intel libva-intel-driver`

### 一些软件

- 中文字体： `noto-sc zh-sourcehanserif-sc-otf`

- 网络管理器： `networkmgr`

- 软件包管理器：`octopkg`

### 修改 sh

chsh 用户名，然后编辑文件即可, 如 sh 一栏改为 /usr/csh 。


### 中文界面

在`/etc/login.conf`中设置：

```
:umask=022:\
:charset=UTF-8:\
:lang=no_NO.UTF-8:
```

### 中文输入法： 

`pkg install zh-fcitx zh-fcitx-libpinyin zh-fcitx-configtool`

输入法配置

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
启用无线：

`sudo ifconfig wlan0 up`



### 自动挂载U盘

注： calibre 安装后会提示挂载USB 的方法

`pkg install automount`

### 对应软件：

- fusefs-ntfs： NTFS 格式

- fusefs-ext2 : 支持读写 ext2, ext3, ext4

- fusefs-simple-mtpfs / fusefs-mtpfs: 安卓类手机内存


## 第三部分

### 使用技巧

- 显示硬件： `vim /var/run/dmesg.boot`

- 登录信息： `vim /var/log/messages`


### 部分软件

vscode  ddrescue plank unrar you-get



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

