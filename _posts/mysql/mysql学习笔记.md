title: mysql学习笔记
id: 789
categories:
  - Mysql
date: 2013-05-21 01:02:05
tags:
---

<!--more-->
```sql
启动:
net stat mysql

停止:
net stop mysql

登录:
mysql -u用户名 -p密码

--查询所有数据库:
show databases

--删除数据库:
drop database mydb;
commit;

--创建数据库:
create database mydb;

--使用数据库:
use mydb;

--查询数据库中所有表:
show tables;

--建表:
create table person
( 
	id varchar(32) not null primary key,
	name varchar(20) not null,
	password varchar(20) not null
)

--显示表结构:
describe person;

--导入.sql命令:
source f:my.sql
my.sql为数据库创建脚本

--查看服务器版本号和当前日期:
select version();
select current_date;

--简单函数:
select PI();

--查看所有用户:
select user();

--查看变量
show variables like 'hav%';

--安装mysql服务
mysqld --install

--移除服务
mysqld --remove

--查看默认引擎
show engines;
```

创建用户
```
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

授权
```
GRANT privileges ON databasename.tablename TO 'username'@'host';
```

更新用户密码
```
UPDATE mysql.user SET password=PASSWORD('newpwd') WHERE user = '' AND host = '';
FLUSH PRIVILEGES;
```

MySQL的JDBC URL格式：
jdbc:mysql//[hostname][:port]/[dbname][?param1=value1][&param2=value2]….

示例：jdbc:mysql://localhost:3306/sample_db?user=root&password=your_password
 
常见参数：
user                       用户名
password                  密码
autoReconnect                  联机失败，是否重新联机（true/false）
maxReconnect              尝试重新联机次数
initialTimeout               尝试重新联机间隔
maxRows                   传回最大行数
useUnicode                 是否使用Unicode字体编码（true/false）
characterEncoding          何种编码（GB2312/UTF-8/…）
relaxAutocommit            是否自动提交（true/false）
capitalizeTypeNames        数据定义的名称以大写表示


在使用mysql执行update的时候，如果不是用主键当where语句，会报如下错误，使用主键用于where语句中正常。
异常内容：Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column To disable safe mode, toggle the option in Preferences -> SQL Queries and reconnect.

MySql运行在safe-updates模式下，该模式会导致非主键条件下无法执行update或者delete命令，执行命令SET SQL_SAFE_UPDATES = 0;

mysql新建用户无法登陆
use mysql ;
delete from user where user=''; 
flush privileges; 