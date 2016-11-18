title: Oracle-JDK安装和配置
categories:
  - Linux
date: 2015-08-01 21:39:31
tags:
---
系统版本:Ubuntu 14.04.2 LTS 64-bit

### JDK下载
Oracle官网下载JDK jdk-7u79-linux-x64.tar.gz
放到/usr/local/java/下

### 解压
```shell
tar -zxvf jdk-7u79-linux-x64.tar.gz
```
### 配置JAVA_HOME和环境变量

```shell
vi /etc/profile
```
在末尾添加:

```shell
export JAVA_HOME=/usr/local/java/jdk1.7.0_79
export PATH=$JAVA_HOME/bin:$PATH
```
### 使环境变量生效

```shell
source /etc/profile
```

eclipse的jre配置
修改eclipse.ini在其头部添加
```
-vm
/usr/local/java/jdk1.7.0_79/jre/bin/java
```
***

插曲:
一开始修改/etc/profile的时候,不小心在$PATH后加了一个i,保存之后系统无法登录
输入正确的密码之后,总是跳回登录界面

解决方法: 在登录界面Ctrl + Alt + F2,使用root登录 vi /etc/profile
因为系统变量配置错误,已经无法在其他目录直接使用vi命令了,ls 命令也不可用
只好进入vi的目录,打开/etc/profile
```shell
cd /usr/share/vim
vi /etc/profile
```
将profile修改正确