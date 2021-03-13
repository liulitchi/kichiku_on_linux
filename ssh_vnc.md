armbiam config: `sudo armbian-config`

## SSH 设置

### 

局域网IP端口扫描： `nmap -sP 192.168.x.y/24`

获取主机IP地址：`cat /proc/net/arp`




## VNC 设置

Kill VNC：
```
vnc list: ps -ef|grep vnc
vncserver -kill :1
```
