title: oracle db-link的使用
categories:
  - oracle
date: 2013-03-03 01:36:44
---

现在需要从不同的oracle数据库中取得数据,使用db-link可以做到
假设从数据库A需要访问数据库B的某张表,建立db-link的步骤如下
**1.更改数据库A的tnsnames.ora文件**

如果数据库是11g,这个文件在appAdministratorproduct11.2.0dbhome_1NETWORKADMIN
增加如下内容:

```
 ORCL_B=
  (DESCRIPTION=
    (ADDRESS=
      (PROTOCOL=TCP)
      (HOST=192.168.1.16)
      (PORT=1521)
    )
    (CONNECT_DATA=
      (SERVER=dedicated)
      (SERVICE_NAME=orcl)
    )
  )
```

说明:

```
1行: ORCL_B:数据库B连接字符串
5,6行: B数据库的ip,port
10行: 数据库B的sid
```

**2.执行SQL创建db-link**

```sql
CREATE PUBLIC DATABASE LINK DB_B
CONNECT TO db_b_username IDENTIFIED BY db_b_pwd
USING 'ORCL_B'
```

说明:

```
DB_B 是db-link的名字
db_b_username,db_b_pwd 是数据库B的用户名,密码
ORCL_B 是tnsnames.ora文件中的数据库B连接字符串
```

**3.使用db-link**
从数据库A中访问数据库B的table_in_b表

```sql
select * from table_in_b@DB_B;
```

db-link详解:[http://www.cnblogs.com/allenlf/archive/2012/04/02/2430265.html](http://www.cnblogs.com/allenlf/archive/2012/04/02/2430265.html "http://www.cnblogs.com/allenlf/archive/2012/04/02/2430265.html")
