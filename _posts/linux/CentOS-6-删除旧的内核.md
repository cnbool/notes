title: CentOS6-删除旧的内核
categories:
  - CentOS
date: 2015-07-31 21:52:31
tags:
---

CentOS升级了一下版本

```shell
yum update
```

查看内核

```shell
[shane@localhost ~]$ rpm -q kernel
kernel-2.6.18-128.el6.x86_64
kernel-2.6.32-504.30.3.el6.x86_64
```

删除老的内核

```shell
rpm -e kernel-2.6.18-128.e15
```