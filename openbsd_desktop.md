## 安装 OpenBSD

待写

## 安装桌面

系统安装完成后重启,OpenBSD 会自动检测无线、显卡和声卡并加载。由于源地址在国外，Ctrl + C 取消。我们先来更新源地址。

### 1 更新源地址

使用 root 账号登录，

 > vim /etc/installurl
 
 注释掉默认源，并添加一行国内源 `https://mirrors.huaweicloud.com/OpenBSD`，保存后退出。
 
 ### 2 安装所需软件
 
 > pkg_add sudo 
 
 > pkg_add vim # 出现多个选项，如果想在未来使用 Gvim，可以选择 vim-gtk3
 
 ### 3 给普通用户添加权限
 
 > visudo
 
 然后添加一行 `$User ALL=(ALL) SETENV=ALL` （请将 $user 替换为你的用户名)，保存后退出。
 
 ### 4 安装桌面和登录管理器
 
 `> exit`退出 root 账号，使用普通账号登录。
 
 
 
