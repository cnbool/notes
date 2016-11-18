title: CentOS6 sudoers config
categories:
  - CentOS
date: 2015-07-31 11:53:31
tags:
---

切换到root用户下

```shell
su
```

打开/etc/sudoers

```shell
vi /etc/sudoers
```

修改/etc/sudoers配置,添加:

```shell
你的用户名	ALL=(ALL)	ALL
```

然后在vi中输入:w!强制保存
这样此用户就可以使用sudo命令了