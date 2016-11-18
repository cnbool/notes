title: Android NDK开发环境搭建
id: 1241
categories:
  - Android
date: 2013-12-27 01:59:01
tags:
---

系统和软件要求
**The Android SDK**
A complete Android SDK installation (including all dependencies) is required.
Android 1.5 SDK or later version is required.

**Supported operating systems**
Windows XP (32-bit) or Vista (32- or 64-bit)
Mac OS X 10.4.8 or later (x86 only)
Linux (32 or 64-bit; Ubuntu 8.04, or other Linux distributions using GLibc 2.7 or later)

**Required development tools**
For all development platforms, GNU Make 3.81 or later is required. Earlier versions of GNU Make might work but have not been tested.
A recent version of awk (either GNU Awk or Nawk) is also required.
For Windows, Cygwin 1.7 or higher is required. The NDK will not work with Cygwin 1.5 installations.

**操作**
1.下载NDK开发包:http://developer.android.com/tools/sdk/ndk/index.html
下载开发包后解压,解压后目录E:/ndk/android-ndk-r9b

2.Windows平台安装cygwin和GNU Make
Windows平台需要下载 [Cygwin](http://www.cygwin.com/ "Cygwin") (1.7或以上版本)
下载安装程序,安装cygwin,Select Packages的时候,需要选中Devel中的make:The GNU version of the 'make' utility
安装完成后,修改配置文件:{安装目录}cygwin64home{用户}.bash_profile
添加一行:
NDK=/cygdrive/E/ndk/android-ndk-r9b
NDK开发环境配置完成.

3.项目NDKPractice
在项目根目录下建立文件夹NDKPractice/jni/
新建类:com.example.ndkpractice.NDKPractice
```java
package com.example.ndkpractice;

public class NDKPractice {
	public native static String getInfo();
}
```
cmd进入到项目位置下,执行命令:
```shell
D:workspaceNDKPractice>javah -classpath bin/classes -d jni com.example.ndkpractice.NDKPractice
```
项目的jni目录下生成头文件:com_example_ndkpractice_NDKPractice.h
