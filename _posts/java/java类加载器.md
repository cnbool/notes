title: java类加载器
id: 721
categories:
  - Java
date: 2013-04-17 21:19:52
tags:
---

Java 中的类加载器大致可以分成两类，一类是系统提供的，另外一类则是由 Java 应用开发人员编写的。系统提供的类加载器主要有下面三个：
**引导类加载器（bootstrap class loader）**：它用来加载 Java 的核心库，是用原生代码来实现的，并不继承自 java.lang.ClassLoader。
**扩展类加载器（extensions class loader）**：它用来加载 Java 的扩展库。Java 虚拟机的实现会提供一个扩展库目录。该类加载器在此目录里面查找并加载 Java 类。扩展库目录:C:Program FilesJavajdk1.6.0_31jrelibext
**系统类加载器（system class loader）**：它根据 Java 应用的类路径（CLASSPATH）来加载 Java 类。一般来说，Java 应用的类都是由它来完成加载的。可以通过 ClassLoader.getSystemClassLoader()来获取它。

源自:[http://www.ibm.com/developerworks/cn/java/j-lo-classloader/](http://www.ibm.com/developerworks/cn/java/j-lo-classloader/)
