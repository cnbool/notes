title: eclipse提示org.w3c.dom.Node下的getTextContent()方法undefined
id: 630
categories:
  - Java
date: 2013-01-27 21:39:44
tags:
---

今天打开之前的一个项目, 发现org.w3c.dom.Node的getTextContent()方法eclipse提示:The method getTextContent() is undefined for the type Node, 百度了一下找到了解决方法:
eclipse 中 如果加入了 其他了xfire 等其他xml解析包的话,使用org.w3c.dom.Node下的getTextContent()方法会出现The method getTextContent() is undefined for the type Node 提示,解决方法如下:

project-->properties->java build path-->order and export 把JRE System 提升到顶部既可,前提记得是java版本是jdk1.5以上

源自:[http://www.cnblogs.com/edwardlauxh/archive/2012/07/01/2572249.html](http://www.cnblogs.com/edwardlauxh/archive/2012/07/01/2572249.html "http://www.cnblogs.com/edwardlauxh/archive/2012/07/01/2572249.html")
