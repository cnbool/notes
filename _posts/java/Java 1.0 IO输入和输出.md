title: Java 1.0 I/O输入和输出
id: 838
categories:
  - Java
date: 2013-05-25 23:12:45
tags:
---

Java 1.0中,类库的设计者限定与输入有关的类应该从InputStrean继承,与输出有关的类应该从OutputStream继承.
**InputStream类型**
ByteArrayInputStream --允许将内存的缓冲区当做InputStream使用
FileInputStream --用户从文件中读取信息
PipedInputStream --产生用于写入相关PipedOutputStream的数据.实现"管道化"概念
SequenceInputStream --将两个或多个InputStream对象转换成单一InputStream
FilterInputStream --抽象类,作为"装饰器"的接口

**OutputStream类型**
ByteArrayOutputStream --在内存中创建缓存区.所有送往"流"的数据都要放置在此缓冲区
FileOutputStream --用于将信息写至文件
PipedOutputStream --写入其中的信息都会自动作为相关PipedInputStream的输出.实现"管道化"概念
FiterOutputStream --抽象类,作为"装饰器"的接口
<!--more-->
**Java I/O中的装饰器模式**
FilterInputStream和FilterOutputStream是用来提供装饰器类接口的两个类.与被装饰的对象同样继承自InputStream和OutputStream(装饰器的必要条件)

**FilterInputStream类型**
DataInputStream --可以从流读取基本数据类型(int, char, long等)
BufferedInputStream --防止每次读取时都进行实际写操作,可以指定缓冲区大小
LineNumberInputStream --跟踪输入流中的行号,getLineNumber()和setLineNumber(int)
PushbackInputStream --可以讲读到的最后一个字符回退

**FilterOutputStream类型**
DataOutputStream --向流中写入基本数据类型(int, char, long等)
BufferedOutputStream --可以避免每次发送数据时都进行实际的写操作
PrintStream --用于产生格式化的输出,处理显示
