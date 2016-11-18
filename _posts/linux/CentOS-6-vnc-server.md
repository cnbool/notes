title: CentOS6 VNC Server
id: 1365
categories:
  - CentOS
date: 2014-04-27 19:27:13
tags:
---

检查是否安装
```shell
# rpm -qa | grep vnc
```

安装
```shell
# yum install tigervnc-server
```

已安装查看安装情况
```shell
# yum info tigervnc-server
```

配置
```shell
# vi /etc/sysconfig/vncservers
```
内容:
```shell
#VNCSERVERS="1:root"
#VNCSERVERARGS[2]="-geometry 800x600 -nolisten tcp -localhost"
```
第一行是登陆用户,格式是"编号:用户名 编号:用户名",用户名对应系统中已存在的用户
例如:"1:root 2:lilei 3:hanmeimei"
第二行800x600为分辨率，-nolisten tcp为阻止tcp包，-localhost监听的hostname
修改配置文件,去掉注释,保存

设置密码
```shell
# vncpasswd
```

操作
```shell
启动服务
# service vncserver start 
停止服务
# service vncserver stop
重启服务
# service vncserver restart
```

加入开机启动
```shell
chkconfig vncserver on
```

vnc监听端口
监听端口=5900 + 用户编号
/etc/sysconfig/vncservers中,编号为1的用户监听端口为5901,编号为2的用户监听端口为5902
防火墙中打开端口
```shell
打开5901,5902
# /sbin/iptables -I INPUT -p tcp --dport 5901 -j ACCEPT
# /sbin/iptables -I INPUT -p tcp --dport 5902 -j ACCEPT
保存
# /etc/rc.d/init.d/iptables save
重启防火墙
# /etc/init.d/iptables restart
```
