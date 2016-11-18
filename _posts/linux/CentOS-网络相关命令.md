title: CentOS网络相关命令
id: 1700
categories:
  - CentOS
date: 2015-06-08 15:38:16
tags:
---

**查看网卡信息**

    ifconfig
    

    **查看某块网卡流量**

    nload eth0
    

    **查看某网络端口状态**

    netstat -anp | grep 9998
    

    参数:

    -a (all)显示所有选项，默认不显示LISTEN相关
    -t (tcp)仅显示tcp相关选项
    -u (udp)仅显示udp相关选项
    -n 拒绝显示别名，能显示数字的全部转化成数字。
    -l 仅列出有在 Listen (监听) 的服務状态

    -p 显示建立相关链接的程序名
    -r 显示路由信息，路由表
    -e 显示扩展信息，例如uid等
    -s 按各个协议进行统计
    -c 每隔一个固定时间，执行该netstat命令。
    

    **查看socket信息**

    ss
    

    参数:

    -h, --help  帮助信息
    -V, --version   程序版本信息
    -n, --numeric   不解析服务名称
    -r, --resolve        解析主机名
    -a, --all   显示所有套接字（sockets）
    -l, --listening 显示监听状态的套接字（sockets）
    -o, --options        显示计时器信息
    -e, --extended       显示详细的套接字（sockets）信息
    -m, --memory         显示套接字（socket）的内存使用情况
    -p, --processes 显示使用套接字（socket）的进程
    -i, --info  显示 TCP内部信息
    -s, --summary   显示套接字（socket）使用概况
    -4, --ipv4           仅显示IPv4的套接字（sockets）
    -6, --ipv6           仅显示IPv6的套接字（sockets）
    -0, --packet            显示 PACKET 套接字（socket）
    -t, --tcp   仅显示 TCP套接字（sockets）
    -u, --udp   仅显示 UCP套接字（sockets）
    -d, --dccp  仅显示 DCCP套接字（sockets）
    -w, --raw   仅显示 RAW套接字（sockets）
    -x, --unix  仅显示 Unix套接字（sockets）
    -f, --family=FAMILY  显示 FAMILY类型的套接字（sockets），FAMILY可选，支持  unix, inet, inet6, link, netlink
    -A, --query=QUERY, --socket=QUERY
          QUERY := {all|inet|tcp|udp|raw|unix|packet|netlink}[,QUERY]
    -D, --diag=FILE     将原始TCP套接字（sockets）信息转储到文件
     -F, --filter=FILE   从文件中都去过滤器信息
           FILTER := [ state TCP-STATE ] [ EXPRESSION ]
    
