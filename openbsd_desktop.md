## 一 安装 OpenBSD

### 下载镜像

进入 [链接](https://mirrors.bfsu.edu.cn/OpenBSD/7.0/amd64/)。如果刻录U盘安装，下载 installXX.img；如果虚拟机，下载 installXX.iso

### 自定义安装

```
p m 查看分区大小
a 增加分区
d 删除分区
z 清空全部分区
q 确认分区
```
默认不要加载桌面，安装完毕后，`vi /etc/sysctl.conf`，设置：`machdep.allowaperture=2`


## 二 安装桌面

系统安装完成后重启，OpenBSD 会自动检测无线、显卡和声卡并下载驱动。静等几分钟，待其自行更新。

### 1 root账户更新源地址

`vi /etc/installurl`

注释掉默认源，改为国内源 `https://mirrors.bfsu.edu.cn/OpenBSD`[^1]

### 2 普通账号获取权限

以 root 账号登录系统，而后新建 `/etc/doas.conf` 文本，打开 `doas.conf`，添加一行 `permit persist :wheel` 。

### 3 进入普通账户操作

#### 3.1 获取 vim

`doas pkg_add vim` #出现多个选项，如果想在未来使用 Gvim，可以选择 vim-gtk3


#### 内核更新

`doas syspatch`

#### 驱动更新

`doas fw_update`

#### 软件升级

`doas pkg_add -u`

#### 修改默认shell

`chsh // chsh -s /usr/local/bin/bash 用户名`


 
### 4 安装桌面和登录管理器
 
#### 4.1 安装 Mate 桌面
 
注意：为防止误操作，本节命令皆为在普通账号下操作。
 
##### 所需安装软件
 
`doas pkg_add slim slim-theme`  [^2]
 
`doas pkg_add elementary-dock mate mate-utils mate-extras` # Mate桌面所需软件
 
##### 配置文件 
 
`cd ~`

`vim .xinitrc` ，添加 `exec mate-session`
 
`doas vim /etc/rc.local` ， 添加 `/usr/local/bin/slim -d`。

`doas vim /etc/rc.conf.local` ，添加
 
```
pkg_scripts="dbus_daemon messagebus"
apmd_flags=-A
multicast_host=YES
```
重启电脑即可进入桌面。
 
#### 4.2 安装 Xfce 桌面
 
注意：为防止误操作，本节命令皆为在普通账号下操作。
 
##### 所需安装软件
 
`doas pkg_add slim`  # slim 为登录管理器
 
`doas pkg_add xfce`  # Xfce 桌面所需软件
 
##### 配置文件 
   
`cd ~` 
 
`vim .xinitrc` ，添加 `exec startxfce4`
 
`doas vim /etc/rc.local` ， 添加 `/usr/bin/local/slim -d`
 
`doas vim /etc/rc.conf.local` ，添加
 
```
pkg_scripts="dbus_daemon messagebus"
apmd_flags=-A
multicast_host=YES
```
重启电脑即可进入桌面。

#### 4.3 Slim 修改主题

注：本节仅涉及 MATE/XFCE ，Gnome 有自己的显示管理器（GDM）。

Slim 的主题文件位于 `/usr/local/share/slim/themes/` 文件夹内，大家可以自己选择喜欢的主题。
这里我们以 **flower2** 主题为例，打开 `/etc/slim.conf`，找到含有 `current_theme` 的一行，将 **default** 改为 **flower2**，保存后即可。

#### 4.4 安装 Gnome 桌面
 
注意：为防止误操作，本节命令皆为在普通账号下操作。
 
 ##### 所需安装软件
 
 `doas pkg_add gdm`  # gdm 为登录管理器
 
 `doas pkg_add gnome-session gnome-terminal nautilus metacity gnome-panel` # Gnome 桌面所需软件
 

##### 配置文件 
 
`cd ~`
 
`vim .xinitrc` ，添加 `exec gnome-session`
 
 `doas vim /etc/rc.conf.local` ，修改内容为：
 
 ```
 gnome_enable=YES
 pkg_scripts="messagebus dbus_daemon gdm"
 multicast_host=YES
```
 重启电脑即可进入桌面。
 
 ## 三 软件管理
 
 查找软件： `pkg_info -Q foo`
 
 安装软件： `pkg_add foo`
 
 升级软件： `pkg_add -iu foo` # i:interactive mode ，u:update
 
 ## 四 中文化
 
 ### 1 安装字体
 
 `doas pkg_add noto-cjk noto-emoji`
 
### 2 安装输入法
 
`doas pkg_add fcitx fcitx-configtool zh-libpinyin`[^3]
 
 ### 3 设置中文
 
 `vim ~/.profile` ，添加以下：
 
 ```
export LANG="zh_CN.UTF-8"
export LC_CTYPE="zh_CN.UTF-8"               
export LC_COLLATE="zh_CN.UTF-8"               
export LC_TIME="zh_CN.UTF-8"                
export LC_NUMERIC="zh_CN.UTF-8"               
export LC_MONETARY="zh_CN.UTF-8"        
export LC_MESSAGES="zh_CN.UTF-8"       
export LC_ALL="zh_CN.UTF-8"

export XIM_PROGRAM=fcitx
export XIM=fcitx
export XMODIFIERS="@im=fcitx"
export QT_IM_MODULE=XIM
export GTK_IM_MODULE=XIM
```
 
 ## 五 主题和图标
 
 ### 1 提前准备
 
 `doas pkg_add git bash wget`
 
### 2 图标安装

```
git clone https://github.com/vinceliuice/Qogir-icon-theme
 
cd Qogir-icon-theme
 
./install.sh
```
### 3 主题安装
```
git clone https://github.com/vinceliuice/Qogir-theme
 
cd Qogir-theme

./install.sh
```
## 六 挂载可移动磁盘
 
### 新建挂载点
 
 `cd ~`
 
 `mkdir media`
 
 `cd media`
 
 `mkdir first second third forth`
 
 ### 查看盘符
 
 使用 `dmesg` 命令来查看新插入的盘符，如格式为 fat32 的 U盘，可能在 openbsd 系统里盘符为 sd1 。
 
 ### 检查分区
 
 如插入的盘符为 `sd1`，则输入 `sudo disklabel sd1` 查看分区情况。如下
 
 ```
 #                size           offset  fstype [fsize bsize   cpg]
  c:         60062500                0  unused                    
  i:         60062244              256   MSDOS    
 ```
 ### 挂载
 
 由上则可知分区为 i ，使用以下命令挂载：
 
 `doas mount /dev/sd1i /$USER/media/first`
 
 
 
### 其它格式
 
OpenBSD 可挂载的外接硬盘格式有 `NTFS`、`ext2/ext3`以及`CD磁盘`等，具体命令可参考如下：

```
doas mount /dev/sd3i /$USER/media/first   # fat32
 
doas mount /dev/sd2k /$USER/media/second  # ntfs
 
doas mount /dev/sd1l /$USER/media/third  # ext2/ext3
 
doas mount /dev/cd0a /$USER/media/forth  # CD
```
  
### 卸载磁盘
 
`doas umount /$User/media/first`
 
 ## 七 无线测试

**OpenBSD** 里的无线网络，配置文件通常是 `hostname.if` ，其中 **if** 为无线驱动名称+序号。如一台笔记本无线型号为 **rtl8188cu** ，**OpenBSD** 下驱动为 **rtwn** ，序号从 0 开始。为了让系统自动加载无线，可打开 `/etc/hostname.rtwn0` 文件 ，而后添加：
```
dhcp 
nwid '无线名称' wpakey '无线密码'
```
保存后即可。

## 八 驱动
 
## 九 补遗
 
### 设置 Shell 中的变量 PS1

`vim ~/.profile`

PS1="[\u@\h \W]$"

### 普通用户关机权限

visudo

取消最后一行shutdown的注释

### ffmpeg 录屏
 
`ffmpeg -f x11grab -s 1600x900 -i :0 -preset ultrafast -crf 10 screen.mp4 `
 
crf 在 wiki 中是质量，0 最好，是无损，51 是最差

### 加载触摸板

打开 `/etc/wsconsctl.conf`， 输入 `mouse.tp.tapping=1`

### 加载线程

打开 `/etc/sysctl.conf`，输入 `hw.smt=1` 。

 
### 参考书籍
 
 - Absolute OpenBSD
 
 - OpenBSD FAQ
 
 ### 技巧问答
 
 ```
 问：firefox 观看优酷和 B 站时提示安装 flash 插件，如何解决？
 答：安装 chromium 并浏览视频，注销后重新进入，firefox 会显示正常。
 问：是什么原理呢？
 答：我也不清楚。
 ```

```
 问：如何解压 7z 或 rar 格式的压缩包
 答：安装 p7zip 和 unrar
 ```

 
[^1]: 此处选择了北外镜像源。也可选择 [清华镜像源](https://mirrors.tuna.tsinghua.edu.cn/OpenBSD)、 [阿里镜像源](https://mirrors.aliyun.com/openbsd) 以及 [南京大学镜像源](https://mirror.sjtu.edu.cn/OpenBSD)。

[^2]: 此处 Linux 和 FreeBSD 用户可用 lightdm 替代，而 OpenBSD 暂未收入该软件。

[^3]: 虽然有 `fcitx-pinyin` 可选，但此处建议改为更易用的 `zh-libpinyin`
