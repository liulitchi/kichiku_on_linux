## 一 安装 OpenBSD

待写

## 二 安装桌面

系统安装完成后重启,OpenBSD 会自动检测无线、显卡和声卡并下载驱动，简直业界良心。虽然源地址在国外，但是速度并没有想象中缓慢，静等几分钟，待其自行更新。

### 1 更新源地址

使用 root 账号登录，

 > vim /etc/installurl
 
 注释掉默认源，改为国内源 `https://mirrors.tuna.tsinghua.edu.cn/OpenBSD`[^1]，保存后退出。
 
 ### 2 安装所需软件
 
 > pkg_add sudo 
 
 > pkg_add vim # 出现多个选项，如果想在未来使用 Gvim，可以选择 vim-gtk3
 
 ### 3 给普通账号添加权限
 
 > visudo
 
 然后添加一行 `$User ALL=(ALL) SETENV: ALL` （请将 $User 替换为你的用户名)，保存后退出。
 
 ### 4 安装桌面和登录管理器
 
 执行`exit`命令退出 root 账号，使用普通账号登录。
 
 #### 4.1 安装 Mate 桌面
 
 注意：为防止误操作，本节命令皆为在普通账号下操作。
 
 #### 所需安装软件
 
 > sudo pkg_add slim  [^2]
 
 > sudo pkg_add mate mate-utils mate-extras # Mate桌面所需软件
 
 > sudo pkg_add firefox chromium thunderbird vlc audacity redshift neofetch # 部分软件，以后可酌量添加
 
 #### 配置文件 
 
 > cd ~
 
 > vim .xinitrc ，添加 `exec mate-session`
 
 > sudo vim /root/.xinitrc ，添加 `exec mate-session`
 
 > sudo vim /etc/rc.local ， 添加 `/usr/bin/local/slim -d`
 
 > sudo vim /etc/rc.conf.local ，添加
 
 ```
 xdm_flags=NO
 sshd_flags=NO
 pkg_scripts="dbus_daemon avahi_daemon"
 dbus_enable=YES
 multicast_host=NO
 ```
 重启电脑即可进入桌面。
 
 #### 4.2 安装 Xfce 桌面
 
 注意：为防止误操作，本节命令皆为在普通账号下操作。
 
 #### 所需安装软件
 
 > sudo pkg_add slim  # slim 为登录管理器
 
 > sudo pkg_add xfce  # Xfce 桌面所需软件
 
 > sudo pkg_add firefox chromium thunderbird vlc audacity bash redshift neofetch # 部分软件，以后可酌量添加
 
  #### 配置文件 
   
 > cd ~ 
 
 > vim .xinitrc ，添加 `exec startxfce4`
 
 > sudo vim /root/.xinitrc ，添加 `exec startxfce4`
 
 > sudo vim /etc/rc.local ， 添加 `/usr/bin/local/slim -d`
 
 > sudo vim /etc/rc.conf.local ，添加
 
 ```
 xdm_flags=NO
 sshd_flags=NO
 pkg_scripts="dbus_daemon avahi_daemon"
 dbus_enable=YES
 multicast_host=NO
 ```
重启电脑即可进入桌面。

 #### 4.3 安装 Gnome 桌面
 
 注意：为防止误操作，本节命令皆为在普通账号下操作。
 
 #### 所需安装软件
 
 > sudo pkg_add gdm  # gdm 为登录管理器
 
 > sudo pkg_add gnome-session gnome-terminal nautilus metacity gnome-panel # Gnome 桌面所需软件，后续可继续添加
 
 > sudo pkg_add firefox chromium thunderbird vlc audacity bash redshift neofetch # 部分软件，以后可酌量添加
 
 #### 配置文件 
 
 > cd ~
 
 > vim .xinitrc ，添加 `exec gnome-session`
 
 > sudo vim /root/.xinitrc ，添加 `exec gmome-session`
 
 
 > sudo vim /etc/rc.conf.local ，修改内容为：
 
 ```
 xdm_flags=NO #xdm 为 openbsd 默认启动器，屏蔽替换为 gdm
 gnome_enable=YES
 gdm_enable=YES
 pkg_scripts="messagebus dbus_daemon avahi_daemon gdm"
 sshd_flags=NO       #ssh设置，需要时可开启
 multicast_host=NO
```
 重启电脑即可进入桌面。
 
 ## 三 软件管理
 
 查找软件： `pkg_info -Q foo`
 
 安装软件： `pkg_add foo`
 
 升级软件： `pkg_add -iu foo` # i:interactive mode ，u:update
 
 ## 四 中文化
 
 ### 1 安装字体
 
 > sudo pkg_add noto-cjk noto-emoji
 
 ### 2 安装输入法
 
 > sudo pkg_add fcitx[^3] libpinyin[^4]
 
 ### 3 设置中文
 
 > vim ~/.profile ，添加以下：
 
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
 
 > sudo pkg_add git bash
 
 ### 2 图标安装
 
 > git clone https://github.com/PapirusDevelopmentTeam/papirus-icon-theme
 
 > cd papirus-icon-theme
 
 > ./install.sh
 
 ### 3 主题安装
 
 > bash

 >git clone https://github.com/vinceliuice/Qogir-theme

 > cd Qogir-theme

 > vim install.sh ，更改第一行(shebang)，将`#!/bin/bash`修改为`#!/usr/local/bin/bash`，而后保存

 > ./install.sh
 
 ## 六 挂载可移动磁盘
 
 使用`dmesg`命令来查看新插入的盘符，如格式为fat32的U盘，可能在openbsd系统里盘符为sd3，则按习惯命名为`sd3i`。
 
 > cd ~
 
 > mkdir media
 
 > cd media
 
 > mkdir first second third forth
 
 ### 挂载`fat32`格式的 U 盘 
 
 > sudo mount /dev/sd3i /home/$USER/media/first
 
 ### 挂载`NTFS`格式的 U 盘 
 
 > sudo mount /dev/sd2k /home/$USER/media/second
  
 ### 挂载`ext2/ext3`格式的 U 盘 
 
  > sudo mount /dev/sd1l /home/$USER/media/third
  
 ### 挂载`CD`磁盘 
 
  > sudo mount /dev/cd0a /home/$USER/media/forth
  
 ### 卸载磁盘
 
 > sudo umount /home/$User/media/first
 
 ## 七 无线测试
 
 ## 八 显卡驱动
 
 ## 九 补遗
 
 ### 参考书籍
 
 ### 技巧问答
 
 ```
 问：firefox观看优酷和B站时提示安装flash插件，如何解决？
 答：安装chromium并浏览视频，注销后重新进入，firefox会显示正常。
 问：是什么原理呢？
 答：我也不清楚。
 ```
 
 ```
 问：为什么两核四线程的电脑，只有两个框框有波动？
 答：OpenBSD会默认检测多核心，似乎线程默认未加载。
 问：那要如何操作呢？
 答：打开 /etc/rc.local ，添加  sysctl hw.smt=1 。
 ```
 
 ```
 问：可以拿OpenBSD来当生产力工具么？
 答：这个……如果使用服务器，可以给 BSD 一个机会。至于桌面环境，目前我还是推荐 Linux 系统。
 问：为什么会这么说？
 答：根据我的实际体验，比如在 OpenBSD 6.7 下用 firefox 浏览B 站视频，480P的分辨率下就发热量巨大，通风口明显感觉烫手;而用Debian 10 观看同样的视频，1080P 的分辨率下也仅仅是少量发热。
 问：那就是说OpenBSD战五渣喽？
 答：也不能这么说，时间是好的武器，五年前的 Linux 比不现今的 Linux，同样的道理，五年后的 OpenBSD 肯定会有不一样的表现，我们应心怀希望。
 问：那你自己使用哪个系统？
 答：我的最爱 Debian + Mate 。
 ```
 
[^1]: 此处选择了清华镜像源，也可选择[华为镜像源](https://mirrors.huaweicloud.com/OpenBSD) 。

[^2]: 此处 Linux 和 FreeBSD 用户可用 lightdm 替代，而 OpenBSD 暂未收入该软件。

[^3]: OpenBSD 二进制包暂未包含`fcitx`配置文件包。

[^4]: 虽然有 `fcitxi-pinyin` 可选，但此处建议改为更易用的 `zh-libpinyin`
