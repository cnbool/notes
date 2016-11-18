title: linux常用命令
categories:
  - linux
date: 2014-08-17 05:18:40
tags:
---

### 防火墙 ###
```shell
开放端口11211
# /sbin/iptables -I INPUT -p tcp --dport 11211 -j ACCEPT
保存
# /etc/rc.d/init.d/iptables save
查看打开的端口：
# /etc/init.d/iptables status

关闭防火墙
# /etc/init.d/iptables stop
重启防火墙
# /etc/init.d/iptables restart
```

------------------------------------------------------------

### 网络 ###
```shell
查看IP
# ifconfig eth0
修改IP
# /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
BOOTPROTO=static
HWADDR=
ONBOOT=yes
TYPE=Ethernet
IPADDR=<你的IP>
NETMASK=255.255.255.0
GATEWAY=192.168.0.1
```
重启network服务
```
service network restart
```

配置DNS
```
vi /etc/resolv.conf
nameserver 223.5.5.5
```

------------------------------------------------------------

### 远程连接配置 ###
```shell
使用SSH
# vi /etc/ssh/sshd_config
将Port,Protocol等注释去掉
修改/etc/hosts.deny 在最后面添加一行：
sshd:All
修改/etc/hosts.allow 在最后面添加一行：
sshd:All
启动SSH
# /etc/init.d/sshd start
在防火墙中开放SSH使用的端口
在Windows上使用PuTTY连接CentOS
```

------------------------------------------------------------

### 文件 ###

删除文件:
```
rm -rf 文件名
```
参数说明:
-rf r:recursion(递归),f:force(强制)

修改文件权限chmod
三种权限:
Read Write Execute
  1    1     1
r=4
w=2
x=1
rwx=7
rw=6

改变文件权限
```
chmod 761 temp.zip
```
root的权限是 rwx（读写和执行权限），
所属用户组权限是 rw (只有读写权限)，
对于其它用户权限是 x(只有只执行权限)

递归改变目录权限
```
chmod 755 -R /usr/local/mydir
```
------------------------------------------------------------

### tar 命令,磁带归档(Tape Archive) ###

tar [-cxtzjvfpPN] 文件/目录

参数：
```
-c ：建立
-x ：解压
-t ：查看

-z:	gzip
-j: bzip2
-v: 进度
-f: 文档名
-p: 属性不变
-P: 使用绝对路径来压缩
-N：比后面接的日期(yyyy/mm/dd)还要新的才会被打包进新建的文件中
--exclude FILE：在压缩的过程中,排除FILE
```

```
tar -cvf /tmp/etc.tar /etc // 打包，不压缩
tar -zcvf /tmp/etc.tar.gz /etc // 打包以 gzip 压缩
tar -jcvf /tmp/etc.tar.bz2 /etc // 打包以 bzip2 压缩

tar -xvf /tmp/etc.tar	// 解包
tar -zxvf /tmp/etc.tar.gz // 解压gzip包
tar -jxvf /tmp/etc.tar.bz2 // 解压bzip2包
```

------------------------------------------------------------

###  linux查看磁盘容量 ###
```
df -lh
```
------------------------------------------------------------

### CentOS永久设置静态路由 ###

查看路由表
```
route -n
```

```
vi /etc/sysconfig/static-routes
```

```
any net 0.0.0.0/0 gw 192.168.1.1
any net 192.168.1.0/24 net mask 255.255.255.0 gw 192.168.1.1
```

```
service network restart
```
------------------------------------------------------------

### 后台执行程序 ###
对于普通程序,启动命令后加 &可以使程序后台运行,但是ssh连接关闭之后程序会停止运行,例:
```
./startup.sh &
```
nohup命令使ssh连接断开之后程序依然在后台运行,例:
```
nohup ./startup.sh &
```
------------------------------------------------------------

### 查看进程 ###
```
ps -ef | grep java
```

------------------------------------------------------------
### 显示文件内容 ###

#### 顺序输出文本文件内容 ####
```
cat [-AbEnTv] filename
```
-E 换行显示$
-T tab显示为^I
-v 显示特殊字符
-n 打印行号，空行有行号
-b 打印行号，空行无行号

-A = -vET

#### 倒序输出文本文件内容 ####
```
tac filename
```

#### 显示文本文件内容,可向后翻页  ####
```
more filename
```
空格键	向下翻页
Enter		向下翻一行
:f 		显示文件名及目前行数
:q 		退出
:b 		往回翻
/字符串	向下搜索

#### 显示文本内容，可向前翻页 ####
```
less filename
```
空格键	向下翻页
pagedown	向下翻页
pageup	向上翻页
/字符串	向下搜索
?字符串	向上搜索

#### 显示文件前几行 ####
```
head [-n number] filename
```

#### 显示后100行前的内容 ####
```
head -n -100 filename
```

#### 显示文件后几行 ####
```
tail [-n number] filename
```

#### 显示前100行以后的内容 ####
```
tail -n +100
```

#### 持续检查并输出最后100行 ####
```
tail -f -n 100 filename
```
ctrl + c 退出
------------------------------------------------------------
## 安装JDK ##
将jdk解压到/usr/local/下：/usr/local/jdk1.7.0_80
在/etc/profile末尾添加：
```
JAVA_HOME=/usr/local/jdk1.7.0_80
PATH=$JAVA_HOME/bin:$PATH
```

执行source /etc/profile 使配置生效


