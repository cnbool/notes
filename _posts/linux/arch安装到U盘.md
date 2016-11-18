title: arch安装到U盘
categories:
  - Linux
date: 2016-06-19 17:19:31
tags:
---
arch安装到U盘

### 分区准备 ###

使用fdisk命令查看挂载的磁盘
```
fdisk -l
```

我的U盘设备为/dev/sdb,使用如下命令管理分区
```
fdisk /dev/sdb
```
进入后按提示可以执行删除新建分区等操作

建好分区之后格式化为ext4格式：
```
mkfs.ext4 -O "^has_journal" /dev/sdb1
```

其中-O "^has_journal" 参数表示不记录日志,这个参数适合U盘或SSD这类写入寿命有限的磁盘，如果使用HDD硬盘可以不加这个参数

挂载根目录
```
mount /dev/sdb1 /mnt
```
---------------------------------------------------------------------

### 源配置 ###

备份mirrorlist
```
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup
```

去掉注释(如果默认情况下Server前没有注释就不需要此步骤)
```
sed -i 's/^#Server/Server/' /etc/pacman.d/mirrorlist.backup
```

找出top 6
```
 rankmirrors -n 6 /etc/pacman.d/mirrorlist.backup > /etc/pacman.d/mirrorlist
```

更新源配置
```
pacman -Syyu
```

---------------------------------------------------------------------

### 安装基本软件包 ###
```
pacstrap -i /mnt base base-devel
```

### fstab ###
```
genfstab -U -p /mnt >> /mnt/etc/fstab
```

---------------------------------------------------------------------

chroot
```
arch-chroot /mnt /bin/bash
```

本地化配置
```
nano /etc/locale.gen
```

```
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
```

生成locale信息
```
locale-gen
```

```
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

时间
```
tzselect
```

将 /etc/localtime 软链接到 /usr/share/zoneinfo/Zone/SubZone，以上海为例:
```
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

建议设置时间标准 为 UTC，并调整 时间漂移:
```
 hwclock --systohc --utc
```

---------------------------------------------------------------------

将系统安装到U盘需要修改/etc/mkinitcpio.conf配置
在执行"mkinitcpio -p linux"之前add the block hook to the hooks array right after udev. 

创建初始 ramdisk 环境
```
mkinitcpio -p linux
```

---------------------------------------------------------------------
设置 Root 密码
```
passwd
```

安装 bootloader
```
pacman -S grub os-prober
grub-install /dev/sdb
grub-mkconfig -o /boot/grub/grub.cfg
```

主机名
```
echo "myhostname" > /etc/hostname
```

并在 /etc/hosts 添加同样的主机名：
```
127.0.0.1	localhost.localdomain	localhost	myhostname
::1		localhost.localdomain	localhost	myhostname
```

---------------------------------------------------------------------

有线网络
如果只用单一且固定的有线网络连接，启动 dhcpcd 服务，interface 是网络接口名
```
systemctl enable dhcpcd@interface.service
```

无线网络
安装 iw, dialog 和 wpa_supplicant, 靠它们连网：
```
pacman -S iw wpa_supplicant dialog
```

也可使用networkmanager（推荐）
```
pacman -S networkmanager
```

---------------------------------------------------------------------

### 安装中文字体  ###
```
pacman -S wqy-zenhei
```

---------------------------------------------------------------------

卸载分区并重启系统
```
exit
umount -R /mnt
reboot
```

---------------------------------------------------------------------

### 网络配置 ###

Check the driver status
```
lspci -k
```

ip link 或 iw dev(无线网络)查到设备名称

启用网卡
```
ip link set wlan0 up
```

---------------------------------------------------------------------

### 使用iw wpa_supplicant连接无线网 ###

无密码WIFI登录
```
iw dev wlan0 connect "your_essid"
```

WEP登录
```
iw dev wlan0 connect "your_essid" key 0:your_key
```

WPA/WPA2登录
```
wpa_supplicant -D nl80211,wext -i wlan0 -c <(wpa_passphrase "your_SSID" "your_key") -B
```

```
iw dev wlan0 link
```

Getting an IP address
```
dhcpcd wlan0
```

---------------------------------------------------------------------
### 使用NetworkManager连接网络 ###

```
systemctl start NetworkManager.service 		#启动NetworkManager
systemctl enable NetworkManager.service 	#设置开机启动NetworkManager
```

nmcli 命令:
To connect to a wifi network:
```
nmcli dev wifi connect <name> password <password>
```

To connect to a wifi on the wlan1 wifi interface:
```
nmcli dev wifi connect <name> password <password> iface wlan1 [profile name]
```

To disconnect an interface:
```
nmcli dev disconnect iface eth0
```

To reconnect an interface marked as disconnected:
```
nmcli con up uuid <uuid>
```

To get a list of UUIDs:
```
nmcli con show
```

To see a list of network devices and their state:
```
nmcli dev
```

To turn off wifi:
```
nmcli r wifi off
```

---------------------------------------------------------------------

### 桌面配置 ###

安装xfce桌面

```
pacman -S xorg
pacman -S xfce4 xfce4-goodies
```

启动桌面

```
startxfce4
```

安装network-manager xfce界面
```
pacman -S network-manager-applet xfce4-notifyd
```

安装fcitx
```
pacman -S fcitx fcitx-configtool fcitx-table-extra fcitx-gtk2 fcitx-gtk3 fcitx-qt4 fcitx-qt5
```

安装fcitx中文输入法
```
pacman -S fcitx-sunpinyin fcitx-googlepinyin
```