## 本节记录一下 FreeBSD 12 开无线的过程，以作查阅，[参考内容](http://www.puchalian.com/freebsd-wireless-networking-basics.html)。

> 我的电脑无线型号 ： RTL8188ce ,接口为 PCI ，FreeBSD 驱动： rtwn

#### 1.修改 vi /boot/loader.conf ,加入以下代码：

```
legal.realtek.license_ack=1    # 
firmware_load="YES"    # 
if_rtwn_pci_load="YES"    # USB 接口可改为 if_rtwn_usb_load="YES",位于 /boot/kernel/if_rtwn_pci.ko
 
wlan_scan_ap_load="YES"
wlan_scan_sta_load="YES"
wlan_wep_load="YES"
wlan_ccmp_load="YES"
wlan_tkip_load="YES"
```

### 2.新建 /etc/wpa_supplicant.conf，添加内容：
```
network={
ssid="替换为你的无线网络名称"
psk="替换为你的无线网络密码"
}
```

### 3. 打开/etc/rc.conf，添加内容：
```
wlans_rtwn0="wlan0"
ifconfig_wlan0="WPA DHCP"
```

### 4. 打开 resolv.conf，添加内容：
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

重启即可识别
