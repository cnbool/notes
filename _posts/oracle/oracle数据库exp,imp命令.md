title: oracle数据库exp,imp命令
categories:
  - oracle
date: 2012-10-11 21:07:44
---

EXP命令可以将oracle数据库的表导出为dmp文件

IMP命令可以将dmp文件导入oracle数据库

**1.EXP:**
有三种主要的方式（完全、用户、表）
1、完全：
EXP SYSTEM/MANAGER BUFFER=64000 FILE=C:FULL.DMP FULL=Y
如果要执行完全导出，必须具有特殊的权限
2、用户模式：
EXP SONIC/SONIC    BUFFER=64000 FILE=C:SONIC.DMP OWNER=SONIC
这样用户SONIC的所有对象被输出到文件中。
3、表模式：
EXP SONIC/SONIC    BUFFER=64000 FILE=C:SONIC.DMP TABLES=(SONIC)
这样用户SONIC的表SONIC就被导出<!--more-->
**2.IMP:**
具有三种模式（完全、用户、表）
1、完全：
IMP SYSTEM/MANAGER BUFFER=64000 FILE=C:FULL.DMP FULL=Y
2、用户模式：
IMP SONIC/SONIC    BUFFER=64000 FILE=C:SONIC.DMP FROMUSER=SONIC TOUSER=SONIC
这样用户SONIC的所有对象被导入到文件中。必须指定FROMUSER、TOUSER参数，这样才能导入数据。
3、表模式：
IMP SONIC/SONIC    BUFFER=64000 FILE=C:SONIC.DMP TABLES=(SONIC)
这样用户SONIC的表SONIC就被导入。
