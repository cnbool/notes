title: mysql常用函数
categories:
  - Mysql
date: 2016-01-19 17:44:05
tags:
---

** 从左开始截取字符串 **
left（str, length） 

** 从右开始截取字符串 **
right（str, length） 

** 截取字符串 **
substring（str, pos） 
substring（str, pos, length） 

** 时间字符串转换 **
SELECT DATE_FORMAT(NOW(), '%Y-%c-%d %H:%i:%s');
SELECT STR_TO_DATE('2016-10-19 08:12:00','%Y-%c-%d %H:%i:%s');