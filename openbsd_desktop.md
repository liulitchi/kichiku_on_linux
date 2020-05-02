## 一 安装 OpenBSD

待写

## 二 安装桌面

系统安装完成后重启,OpenBSD 会自动检测无线、显卡和声卡并加载，简直业界良心。虽然源地址在国外，但是速度并没有想象中的缓慢，静待几分钟，让其自行更新驱动。

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
 
 > sudo pkg_add mate mate-extras # Mate 桌面所需软件
 
 > sudo pkg_add firefox chromium thunderbird vlc audacity bash redshift neofetch # 部分软件，以后可酌量添加
 
 #### 配置文件 
 
 > cd ~
 
 > vim .xinitrc ，添加 `exec mate-session`
 
 > sudo vim /root/.xinitrc ，添加 `exec mate-session`
 
 > sudo vim /etc/rc.local ， 添加 `/usr/bin/local/slim -d`
 
 > sudo vim /etc/rc.conf.local ，添加
 
 ```
 pkg_scripts="dbus_daemon avahi_daemon"
 dbus_enable=YES
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
 pkg_scripts="dbus_daemon avahi_daemon"
 dbus_enable=YES
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
 sshd_flag=NO       #ssh 设置，如非必要可关闭
 multicast_host=YES #这一行不知道啥意思
```
 重启电脑即可进入桌面。
 
 ## 三 软件管理
 
 查看软件： `pkg_info -Q pkg`
 
 安装软件： `pkg_add foo`
 
 删除缓冲： `pkg_delete -a`
 
 ## 四 中文化
 
 ## 五 主题和图标美化
 
 ## 六 挂载U盘
 
 ## 七 无线测试
 
 ## 八 显卡驱动
 
[^1]: 此处选择了清华镜像源，也可选择[华为镜像源](https://mirrors.huaweicloud.com/OpenBSD) 。
