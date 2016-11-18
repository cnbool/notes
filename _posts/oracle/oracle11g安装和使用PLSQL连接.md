title: oracle11g安装和使用PLSQL连接
categories:
  - oracle
date: 2016-10-29 15:54:24
---

## Oracle 11g R2下载安装##

windows版:
http://www.oracle.com/technetwork/database/enterprise-edition/downloads/112010-win64soft-094461.html

下载文件:
win64_11gR2_database_1of2.zip
win64_11gR2_database_2of2.zip

下载后在服务器解压安装
oracle数据库home目录:D:\app\用户名\product\11.2.0\dbhome_1
配置文件所在目录:D:\app\用户名\product\11.2.0\dbhome_1\NETWORK\ADMIN

## oracle client下载安装##

上文链接中包含oracle客户端win64_11gR2_client.zip
下载后安装到需要连接oracle的机器上
oracle client安装后目录:D:\app\用户名\product\11.2.0\client_1
配置文件所在目录:D:\app\用户名\product\11.2.0\client_1\network\admin

## PL/SQL Developer下载安装##

https://www.allroundautomations.com/
下载PL/SQL Developer 64 bit Version 11.0.6
同oracle client安装在同一台机器上

## PL/SQL Developer配置 ##

PL/SQL Developer连接Oracle前需要配置oracle client
复制oracle服务器的
D:\app\用户名\product\11.2.0\dbhome_1\NETWORK\ADMIN\tnsnames.ora
至oracle client所在机器的
D:\app\用户名\product\11.2.0\client_1\network\admin\tnsnames.ora
修改tnsnames.ora内容,将HOST的值修改为服务器端IP,例如：

```
ORCL =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.122)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )
```

打开PL/SQL Developer即可登录