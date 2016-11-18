title: CentOS6 FTP Server
id: 1372
categories:
  - CentOS
date: 2014-09-10 21:51:31
tags:
---

安装
```shell
# yum install vsftpd
```

操作
```shell
启动
# service vsftpd start
关闭
# service vsftpd stop
重启
# service vsftpd restart
```

```shell
开放端口21
# /sbin/iptables -I INPUT -p tcp --dport 21-j ACCEPT
保存
# /etc/rc.d/init.d/iptables save
```

# vi /etc/vsftpd/vsftpd.conf 主配置文件
做如下修改:
```shell
chroot_local_user=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd.chroot_list
```
说明:
chroot_local_user用于指定用户列表文件中的用户是否允许切换到上级目录,默认值为NO
chroot_local_user=YES时chroot_list_file中用户可以切换到上级目录,其他用户不可以
chroot_local_user=NO时chroot_list_file中用户不可以切换到上级目录,其他用户可以
chroot_list_enable设置是否启用chroot_list_file配置项指定的用户列表文件,默认值为NO
chroot_list_file用于指定用户列表文件

添加FTP本地用户ftpuser
```shell
# adduser -d /opt/test_ftp -g ftp -s /sbin/nologin ftpuser
```

用户设置密码
```shell
# passwd ftpuser
```

CentOS系统安装了SELinux，默认没有开启FTP的支持
```shell
查看SELinux设置
# getsebool -a|grep ftp

设置ftp_home_dir,allow_ftpd_full_access 
# setsebool -P ftp_home_dir 1
# setsebool -P allow_ftpd_full_access 1
```

开机启动
```shell
# chkconfig vsftpd on
查看开机启动
# chkconfig --list|grep vsftpd
```
