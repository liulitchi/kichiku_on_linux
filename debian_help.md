## Debian install


## Debian after install

### 修改源文件

debian 10 的代码名 buster ，debian 11 的代码名 bullseye 。

> 


### 删除部分软件包

- firefox 的语言包。

- 数量众多的 task-%language-desktop

- 没有硬件的显卡驱动： `xserver-xorg-video-ati` `xserver-xorg-video-amdgpu` `xserver-xorg-video-radeon` 等。



### debian Mate 自动登录[来源](https://ubuntu-mate.community/t/auto-login-to-the-desktop/60)

  debian Mate 的登录管理器为 lightDM， 修改方式为打开`/usr/share/lightdm/lightdm.conf.d/60-lightdm-gtk-greeter.conf `文件，没有就新建一个。
  
  添加 三行：
  
  ```
[SeatDefaults]
greeter-session=lightdm-gtk-greeter
autologin-user=你的用户名
```

### dmesg 出现乱码[来源](http://forums.debian.net/viewtopic.php?t=8457)

出现几百行`[46092.005476] evbug: Event. Dev: input5, Type: 1, Code: 272, Value: 0`
解决方式：

  进入`/lib/modules/<kernelversion>/kernel/drivers/input/`
  
  修改 `evbug.ko`文件，如改为`evbug.off`。保存后重启。
  
  
