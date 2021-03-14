[armbian 中科大镜像下载](https://mirrors.ustc.edu.cn/armbian-dl/)

armbiam 配置: `sudo armbian-config`

## SSH 设置

### 

局域网IP端口扫描： `nmap -sS 192.168.x.y/24`

获取主机IP地址：`cat /proc/net/arp`




## VNC 设置

杀死 VNC 进程：
```
ps -ef|grep vnc
vncserver -kill :1
```
