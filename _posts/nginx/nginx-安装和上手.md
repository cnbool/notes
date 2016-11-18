title: nginx-安装和上手
categories: nginx
date: 2016-05-15 19:42:00
---

##  CentOS 6.7 安装nginx ##

HTTP rewrite module requires the PCRE library

### 查询pcre是否已安装 ###

```
$ rpm -qa pcre
pcre-7.8-7.el6.x86_64
```

也可通过查询pcre版本得知是否安装
```
$ pcretest -C
PCRE version 7.8 2008-09-05
```

如果想卸载，pcre卸载方法
```
$ rpm -e  --nodeps pcre-7.8-7.el6.x86_64
```

目前系统中已经安装了7.8版本的pcre，如未安装需要安装pcre
```
$ yum install -y pcre pcre-devel
```

### 添加nginx源 ###

创建 /etc/yum.repos.d/nginx.repo 内容如下:
```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/OS/OSRELEASE/$basearch/
gpgcheck=0
enabled=1
```

替换"OS"为“rhel” 或 “centos”
“OSRELEASE” 替换为 “5”, “6”, 或 “7”

### 安装 ###
```
$ yum install nginx
```

安装成功后查看nginx安装位置
```shell
$ whereis nginx
nginx: /usr/sbin/nginx /etc/nginx /usr/lib64/nginx /usr/share/nginx
```

nginx 配置文件位置
/usr/local/nginx/conf, /etc/nginx, or /usr/local/etc/nginx

### 启动 ###
```shell
$ nginx
```

### nginx命令 ###
```shell
$ nginx -s signal
```

Where signal may be one of the following:

stop — fast shutdown
quit — graceful shutdown
reload — reloading the configuration file
reopen — reopening the log files

优雅的退出
```shell
$ nginx -s quit
```
注意：只有在启动nginx的账户下可以执行此命令

根据nginx的主进程pid用kill命令将其关闭
```
$ kill -s QUIT <pid>
```
nginx.pid文件记录主进程pid，位于/usr/local/nginx/logs 或 /var/run

重新加载配置文件
```
nginx -s reload
```
当主线程接收到重新加载配置文件命令后，它会检查配置文件的语法并尝试应用其支持的配置。
如果这个步骤成功了，主线程开始启动新的工作进程并请求老的工作线程关闭。如果失败，主线程回滚更改，以配置更改前的方式运行。
老的工作进程收到关闭命令后停止接收新的连接请求，继续处理进行中的请求，直到所有进行中的请求处理完毕，全部老的工作线程退出。

获得运行中的nginx进程列表
```
ps -ax | grep nginx
```

### 配置文件的结构 ###

nginx的模块由配置文件中定义的指令来控制。指令分为两种：简单指令和块指令。
简单的指令：名称+空格+参数，以分号结尾。
块指令：名称+空格+参数，以{}包含的附加指令结尾。
如果一个指令块可以在括号内有其他指令，那它被称为上下文,例如：events, http, server, and location

配置文件中在任何上下文之外的指令被认为在main上下文中。
events和http指令在main上下文中，server 在 http 中, location 在 server 中。


### 提供静态内容 ###

web server的一个重要任务是返回图片或静态HTML。例子：
创建/data/www目录放入index.html
创建/data/images目录放入一些图片

打开nginx配置文件，默认配置文件已经包含一些server块例子，注释掉它，写一个新的server:
```
server {
    location / {
        root /data/www;
    }

    location /images/ {
        root /data;
    }
}
```

请求http://localhost/images/example.png nginx 将会返回/data/images/example.png。
如果此文件不存在，nginx会返回404错误。
如果请求的URI不是以 /images/ 开始，请求会被映射到 /data/www 目录。
例如 http://localhost/some/example.html 会返回 /data/www/some/example.html

一般情况下，配置文件可以包含几个server块，分别监听不同的端口且server names也不同。
当nginx决定哪个server处理一个请求时，它会检查请求头中的URI与server块中location的参数是否匹配。
如果一个请求匹配多个location块，nginx会选择路径最长的那个。
如果nginx没有按照预期工作，可以从/usr/local/nginx/logs 或 /var/log/nginx中的access.log 和 error.log里查找原因。

### 设置一个简单的代理服务器 ###

我们将配置一个基本的代理服务器，从本机返回对图片的请求，将其他请求发往被代理服务器。
在这个例子中，proxied server和proxy server都配置在一个nginx实例中。

proxied server配置：
```
server {
    listen 8080;
    root /data/up1;

    location / {
    }
}
```

监听8080端口，将所以请求映射到/data/up1
注意：以上配置中的root指令在server上下文中，当location块不包含自己的root指令时可以这么干。

proxy server配置：
```
server {
    location / {
        proxy_pass http://localhost:8080;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```

proxy server会过滤以 .gif, .jpg, 或 .png 结尾的请求，并映射到 /data/images，将其他请求送往proxied server。

