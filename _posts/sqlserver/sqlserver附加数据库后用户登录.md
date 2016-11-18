title: sqlserver附加数据库后用户登录
categories:
  - SqlServer
date: 2016-06-17 15:37:19
tags:
---
Sqlserver 2008
数据库:mydb
用户名:xiaoming

将此数据库文件复制到其他机器，附加此数据库后需要新建登录账户：
在'安全性'-'登录名'中新建登录用户mydb
将此用户映射到mydb，数据库角色成员身份：db_owner
点击确定后提示：服务器主体'xiaoming'已存在。(MicrosoftSQL Server，错误:15025)

执行如下sql可将登录名'xiaoming'与数据库中的'xiaoming' 关联起来

```sql
USE mydb
GO
sp_change_users_login 'update_one', 'xiaoming', 'xiaoming'
```
