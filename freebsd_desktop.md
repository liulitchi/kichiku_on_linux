## 第一部分
    
FreeBSD 12.1-release 安装完成后，默认界面是黑黢黢的终端。我们试着安装 Mate 桌面。
    
第一步，改源。

先用root账号登录系统。FreeBSD 12.1 默认安装了 vi 和 ee 两个文本阅读器，选择一个适合的

`vi /etc/pkg/FreeBSD.conf`

将文本内 enable 后的 yes 改为 no。这一步是禁止官方源。
    
 
 然后新建源文本:
 
 > mkdir -p /usr/local/etc/pkg/repos/  #新建文件夹

`vi /usr/local/etc/pkg/repos/FreeBSD.conf`， 这一步是新建配置文件 FreeBSD.conf
  
打开后，添加以下内容：
    
```
FreeBSD: {
  url: "pkg+http://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/quarterly",
 }
```
<s>修改operation timed out：</s>

<s>打开 /usr/local/etc/pkg.conf,  查找 fetch_entry 一行，取消注释，并改为 fetch_entry = 10 ;同理 fetch_timeout = 200(或者为 fetch_time)</s>

保存文本后，就可以开始更新源了
```    
pkg update 
    
pkg upgrade 
```    
    
安装必要软件： sudo 和 vim
    
`pkg install sudo vim`
   
   
vi打开sudo:
    
`visudo` 
    
在 root ALL=(ALL) ALL 下，添加一行 `用户名 ALL=(ALL) ALL`,比如你的用户名为 xiaowang, 这一行就是 `xiaowang ALL=(ALL) ALL`
保存后退出。
    
开始安装 Mate 桌面：
    
`pkg install mate mate-desktop xorg` 
    
这一步大概要安装近三百个软件，需要耗费大量时间，你可以去忙别的事情了。不过要检查一下是否有超时现象，到时候再执行一遍本命令，继
