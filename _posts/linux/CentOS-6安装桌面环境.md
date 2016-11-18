title: CentOS-6-安装桌面环境
categories:
  - CentOS
date: 2015-08-08 22:42:31
tags:
---

## 安装桌面环境

CentOS-6.7-x86_64-minimal.iso不带桌面环境，通过yum安装

```shell
yum groupinstall "Desktop"
yum groupinstall "X Window System"
yum groupinstall "Chinese Support"
```

## 开机进入图形界面 
编辑/etc/inittab文件：

id:3:initdefault
修改为
id:5:initdefault


## 配置输入法
安装了中文支持后，输入法框架是ibus
进入System->Preferences->Input Method设置

## 安装字体
可以从Windows的C:\Winodws\Fonts 里找需要的字体
放入centos的 /usr/share/fonts/目录下新建的文件夹，一种字体建立一个文件夹存放

进入字体文件夹 

```shell
mkfontscale
mkfontdir
fc-cache -fv
```
字体安装完成

如果操作过程中提示mkfontscale: command not found

```shell  
yum install mkfontscale
```

字体安装好以后，在System->Perferences->Appearance里可以选择字体
