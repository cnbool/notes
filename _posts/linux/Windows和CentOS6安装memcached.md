title: Windows和CentOS6安装memcached
id: 1342
categories:
  - CentOS
date: 2014-04-24 01:18:47
tags:
---

**一.Windows系统**
下载memcached.exe,存放位置D:program filesmemcachedmemcached.exe
cmd下运行如下命令创建memcached服务:
```shell
sc create "memcached 11211" start= auto binPath= "D:/program files/memcached/memcached.exe -d runservice -m 256 -p 11211" DisplayName= "memcached 11211"
sc create "memcached 11212" start= auto binPath= "D:/program files/memcached/memcached.exe -d runservice -m 256 -p 11212" DisplayName= "memcached 11212"
sc create "memcached 11213" start= auto binPath= "D:/program files/memcached/memcached.exe -d runservice -m 256 -p 11213" DisplayName= "memcached 11213"
```
删除服务
```shell
sc delete "memcached 11211"
sc delete "memcached 11212"
sc delete "memcached 11213"
```
<!--more-->
**二.CentOS6**
**1.首先安装libevent**
http://libevent.org/下载libevent-1.4.14b-stable.tar.gz到/usr/local/src/
```shell
# tar -xzvf libevent-1.4.14b-stable.tar.gz
# cd libevent-1.4.14b-stable
# ./configure –prefix=/usr/local
# make
# make install
```

**2.安装memcached**
http://memcached.org/下载memcached-1.4.18.tar.gz到/usr/local/src/
```shell
# tar -xzvf memcached-1.4.18.tar.gz
# cd memcached-1.4.18
# ./configure –with-libevent=/usr/local
# make
# make install
```

**3.启动memcached**
```shell
# /usr/local/bin/memcached -d -m 512 -u root -p 11211 -P /tmp/memcached.pid
```
启动参数说明：
   -d  选项是启动一个守护进程，
   -m  分配给Memcache使用的内存数量，单位是MB，默认64MB
   -M  内存耗尽的时候返回错误
   -u  运行Memcache的用户，如果当前为root 的话，需要使用此参数指定用户。
   -l  监听的服务器IP地址，默认为所有网卡。
   -p  设置Memcache的TCP监听的端口，最好是1024以上的端口
   -c  选项是最大运行的并发连接数，默认是1024
   -P  设置保存Memcache的pid文件
   -f  块大小增长因子，默认是1.25
   -I  Override the size of each slab page. Adjusts max item size(1.4.2版本新增)

停止Memcache进程：
```shell
# kill `cat /tmp/memcached.pid`
```
**

_如果要开机启动将启动memcached的命令加入/etc/rc.d/rc.local_
<!--more-->
<strong>4.开放memcached使用的端口(只本机访问memcached时此步骤可省略)**
```shell
开放端口11211
# /sbin/iptables -I INPUT -p tcp --dport 11211 -j ACCEPT
保存
# /etc/rc.d/init.d/iptables save
查看打开的端口：
# /etc/init.d/iptables status

相关命令:
关闭防火墙
# /etc/init.d/iptables stop
重启防火墙
# /etc/init.d/iptables restart
```

**5.memcached命令**
如未安装telnet,先安装
```shell
# yum install telnet
```

telnet连接memcached,连接成功后,输入stats,查看memcached状态信息:
[code lang="shell" highlight="1,5"]
[root@localhost sbin]# telnet 127.0.0.1 11211
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
stats
STAT pid 699    --memcache服务器的进程ID
STAT uptime 182    --服务器已经运行的秒数
STAT time 1398329401    --服务器当前的unix时间戳
STAT version 1.4.18    --memcache版本
STAT libevent 1.4.14b-stable    --libevent版本
STAT pointer_size 32    --当前操作系统的指针大小（32位系统一般是32bit,64就是64位操作系统）
STAT rusage_user 0.007998    --进程的累计用户时间
STAT rusage_system 0.147977    --进程的累计系统时间
STAT curr_connections 10    --当前连接数
STAT total_connections 11    --从memcached服务启动到当前时间，系统连接的总数
STAT connection_structures 11  --服务器分配的连接构造数
STAT reserved_fds 20
STAT cmd_get 0    --累积获取数据的数量
STAT cmd_set 0    --累积保存数量
STAT cmd_flush 0    --flush次数
STAT cmd_touch 0    --touch次数
STAT get_hits 0    --命中次数
STAT get_misses 0    --未命中次数
STAT delete_misses 0    --delete未命中次数
STAT delete_hits 0    --delete命中次数
STAT incr_misses 0    --incr未命中次数
STAT incr_hits 0    --incr命中次数
STAT decr_misses 0    --decr未命中次数
STAT decr_hits 0    --decr命中次数
STAT cas_misses 0    --cas未命中次数
STAT cas_hits 0    --cas命中次数
STAT cas_badval 0    --使用擦拭次数
STAT touch_hits 0    --touch命中次数
STAT touch_misses 0    --touch未命中次数
STAT auth_cmds 0    --认证命令处理的次数
STAT auth_errors 0    --认证失败数目
STAT bytes_read 7    --读取字节数
STAT bytes_written 0    --写入字节数
STAT limit_maxbytes 268435456    --分配给memcached的内存大小
STAT accepting_conns 1    --服务器是否达到过最大连接0/1
STAT listen_disabled_num 0    --失效的监听数
STAT threads 4    --当前线程数
STAT conn_yields 0    --连接操作主动放弃数目
STAT hash_power_level 16
STAT hash_bytes 262144
STAT hash_is_expanding 0
STAT malloc_fails 0
STAT bytes 0    --当前存储占用的字节数
STAT curr_items 0    --当前存储条数
STAT total_items 0    --启动以来存储的数据总条数
STAT expired_unfetched 0
STAT evicted_unfetched 0
STAT evictions 0
STAT reclaimed 0
STAT crawler_reclaimed 0
END
```

存储命令
[code lang="text"]
<command name> <key> <flags> <exptime> <bytes>
<data block>
```

[code lang="text"]
<command name> 	set/add/replace
<key> 	查找关键字
<flags> 	客户机使用它存储关于键值对的额外信息
<exptime> 	该数据的存活时间，0表示永远
<bytes> 	存储字节数
<data block> 	存储的数据块（可直接理解为key-value结构中的value）
```
