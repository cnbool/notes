title: Ubuntu安装fcitx输入法
categories:
  - Linux
date: 2015-08-02 13:04:31
tags:
---
系统版本:Ubuntu 14.04.2 LTS 64-bit

打开Unbuntu Software Center,搜索fcitx

安装Fcitx Start Input Method,会一并安装依赖项,安装好以后修改系统输入法

System Setting -> Language Support -> Keyboard input method system: fcitx

安装拼音输入法

```shell
sudo apt-get install fcitx-pinyin
```
System Setting -> Text Entry 点击+ 添加Chinese

在右上角有个键盘的图标,右键 Configure,在打开的窗口的Input Method标签中添加Pinyin
