## Debian install


## Debian after install

### 修改源文件

添加 contrib 以及 non-free

debian 11 代号为 bullseye ，debian 12 代号为 bookworm 。

> 


### 删除部分软件包

- firefox 的语言包。

- 数量众多的 task-%language-desktop

- 没有硬件的显卡驱动： `xserver-xorg-video-ati` `xserver-xorg-video-amdgpu` `xserver-xorg-video-radeon` 等。



### debian Mate 自动登录[来源](https://ubuntu-mate.community/t/auto-login-to-the-desktop/60)

  debian Mate 的登录管理器为 lightDM， 修改方式为打开`/usr/share/lightdm/lightdm.conf.d/01_debian.conf`。
  
  修改为以下：
  
  ```
[Seat:*]
greeter-session=lightdm-greeter
greeter-hide-users=false
autologin-user=你的用户名
```

### dmesg 出现乱码[来源](http://forums.debian.net/viewtopic.php?t=8457)

出现几百行`[46092.005476] evbug: Event. Dev: input5, Type: 1, Code: 272, Value: 0`
解决方式：

  进入`/lib/modules/<kernelversion>/kernel/drivers/input/`
  
  修改 `evbug.ko`文件，如改为`evbug.off`。保存后重启。
  
  
### 多语言支持，如中英日：
  
 sudo dpkg-reconfigure locales ，出现选项框后添加。
 
 
### 添加32位依赖
 
 > sudo dpkg --add-architecture i386 
 
 >  sudo apt update && apt install wine32 
 
 ### 取消32位平台
 
> sudo apt remove `dpkg --get-selections |grep i386 |awk '{print $1}'`
 
> sudo dpkg --remove-architecture i386
 

## virtualbox 无法启动

添加内核对应头文件(headers)

## vim 配置

> vim .vimrc

```
set guifont=Courier\ 20   " gvim 字体大小

syntax on                   " 语法高亮

set hlsearch                " 搜索高亮

set tabstop=4               " Tab 空格长度

set autoindent              " 自动缩进

colorscheme desert          " 背景主题

set nu                      " 显示行号
```

## 添加路径（两种方式， .profle 或 .bashrc)

> source $HOME/.local/env 

or

```
 export PATH="$HOME/.local/bin:$PATH"
```
建议在 .bashrc 里添加

## 更改 grub 背景图片

    选取合适的图片： sudo cp xxx.png /usr/share/backgrounds/

    编辑 grub 配置文件 ： sudo vim /etc/default/grub

    添加一行： GRUB_BACKGROUND="/usr/share/backgrounds/xxx.png"

    更新 gurb 配置： sudo update-grub

    重启系统： sudo reboot

## [ffmpeg 录屏](https://trac.ffmpeg.org/wiki/Capture/Desktop)


- Use the x11grab device:

`ffmpeg -video_size 1024x768 -framerate 25 -f x11grab -i :0.0+100,200 output.mp4`

This will grab the image from desktop, starting with the upper-left corner at x=100, y=200 with a width and height of 1024⨉768.

- If you need audio too, you can use ALSA (see Capture/ALSA for more info):

`ffmpeg -video_size 1024x768 -framerate 25 -f x11grab -i :0.0+100,200 -f alsa -ac 2 -i hw:0 output.mkv`

- Or the pulse input device (see Capture/PulseAudio for more info):

`ffmpeg -video_size 1024x768 -framerate 25 -f x11grab -i :0.0+100,200 -f pulse -ac 2 -i default output.mkv`


## 安装谷歌地球

> wget https://dl.google.com/dl/earth/client/current/google-earth-pro-stable_current_amd64.deb

> sudo dpkg -i google-.deb

## mate network-manager

> sudo apt install network-manager-gnome


[from](https://github.com/electron/electron/issues/17972)


- [ ] 未完成
- [X] 已完成
