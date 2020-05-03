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
 
 然后添加一行 `$User ALL=(ALL) SETENV: ALL` （请将 $user 替换为你的用户名)，保存后退出。
 
 ### 4 安装桌面和登录管理器
 
 执行`exit`命令退出 root 账号，使用普通账号登录。
 
 #### 4.1 安装 Mate 桌面
 
 注意：为防止误操作，本节命令皆为在普通账号下操作。
 
 #### 所需安装软件
 
 > sudo pkg_add slim  # slim 为登录管理器，替换选项为 lightdm
 
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
 multicast_host=YES
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
 multicast_host=YES
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
 multicast_host=YES #这一行不知道啥意思
```
 重启电脑即可进入桌面。
 
 ## 三 软件管理
 
 查找软件： `pkg_info -Q foo`
 
 安装软件： `pkg_add foo`
 
 删除缓冲： 
 
 ## 四 中文化
 
 ### 1 安装中文字体
 
 > sudo pkg_add noto-cjk noto-emoji
 
 ### 2 安装 fcitx
 
 > sudo pkg_add fcitx fcitx-pinyin libpinyin
 
 ### 3 设置中文
 
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
 
 ## 六 挂载U盘
 
 ## 七 无线测试
 
 ## 八 显卡驱动
 
 ## 九 技巧问答
 
 ```
 问：firefox观看优酷和B站时提示安装flash插件，如何解决？
 答：安装chromium并浏览视频，注销后重新进入，firefxo会显示正常。
 问：是什么原理呢？
 答：我也不清楚。
 ```
 
 ```
 问：为什么firefox搜不到插件？
 答：很抱歉，openbsd暂不支持。
 问：可是，FreeBSD里面就可以用啊？
 答：这个……或许可以联系原作者，或者自己造轮子。
 问：没得到任何有用信息
 答：汗
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
 
 
[^1]: 此处选择了清华镜像源，也可选择[华为镜像源](https://mirrors.huaweicloud.com/OpenBSD) 。
