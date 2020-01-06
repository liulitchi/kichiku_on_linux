## 第一部分
    
默认安装了 FreeBSD 12.1 后，默认界面是黑黢黢的终端。试着安装一下 Mate 桌面，并进行一些后续操作，做一下记录，以供查阅。
    
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
修改operation timed out：
打开 /usr/local/etc/pkg.conf,  查找 fetch_entry 一行，取消注释，并改为 fetch_entry = 10 ;同理 fetch_timeout = 200(或者为 fetch_time)

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
    
添加中文字体： pkg install noto-sc
    
安装网络管理器： pkg install networkmgr 
    
安装中文输入法： pkg install zh-fcitx zh-fcitx-sunpinyin
    
根据安装后的提示，执行 vim .xinitrc，添加以下内容
    
```
export XMODIFIERS='@im=fcitx'
export GTK_IM_MODULE=fcitx
export GTK3_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
```
    
### 无线模块
    
查看无线硬件型号： > pciconf -lv
    
我的笔记本显示出无线型号为 RTL8188CU，查阅手册发现，开启 rtwn 模块即可开启无线，步骤如下（其它无线模块大同小异）：
    
打开 `/boot/loader.conf` ,添加以下内容：

```   
if_rtwn_load="YES"

if_rtwn_pci_load="YES"

legal.realtek.license_ack=1
```
    
打开 `/etc/rc.conf`，添加以下内容

```   
wlans_rtwn0="wlan0"

ifconfig_wlan="WPA DHCP"
```

### 挂载U盘

说实话，这一段是懵的，我也不知道做了哪些，就手动挂载了。

#### fat32 格式的u盘。

运行 dmesg ，确定它的盘符。或者

ls /dev/da* ,会出现以下内容：

`/dev/da0    /dev/da0s1  /dev/da0s1a /dev/da0s1b /dev/da1`

则确定了 `da1` 是我要挂载的U盘，运行

`mount_msdosfs /dev/da1 /media/da1`即可挂载。

#### ext4 格式的U盘

安装识别 ext 格式的软件： pkg install fusefs-ext2

确定u盘的盘符是 da1s1 后， 运行ext4fuse /dev/da1s1 /media/da1s1

然而结果是可以显示盘符，但是无法看到U盘内容。

于是， 灵光一闪：
caja /media/da1s1

居然显示了

同理， Sata 硬盘的格式为 ada*






    
    
    
    
    
    
    
    