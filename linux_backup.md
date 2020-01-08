## 更改 grub 背景图片

- 选取合适的图片： sudo cp xxx.png  /usr/share/backgrounds/

- 编辑 grub 配置文件 ： sudo vim /etc/default/grub

  添加一行： `GRUB_BACKGROUND="/usr/share/backgrounds/xxx.png"`
  
- 更新 gurb 配置： sudo update-grub

- 重启系统：  sudo reboot
