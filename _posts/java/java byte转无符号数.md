title: java byte转无符号数
id: 1669
categories:
  - Java
date: 2015-03-30 12:15:54
tags:
---

**背景**

解析byte格式的信令时候有两个这样的字段

MSC信令点编码(OPC) 3 BYTE

BSC信令点编码（OPC） 3 BYTE

这两个字段是国内信令点编码(24位),需要解析成这样:4-254-39

**java 中的byte**

国内信令点编码范围是0~255

java中的byte类型是8位有符号的,取值范围-128~127

**byte中的值转化为无符号数**

    byte b = -3;
    int i = b & 0xff;

此时i中的值即为byte所含值的无符号数
