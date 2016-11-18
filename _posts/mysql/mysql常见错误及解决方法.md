title: mysql常见错误及解决方法
categories:
  - Mysql
date: 2016-01-19 17:51:05
tags:
---

Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column To disable safe mode, 
toggle the option in Preferences -> SQL Queries and reconnect.

解决方法:
SET SQL_SAFE_UPDATES = 0;


