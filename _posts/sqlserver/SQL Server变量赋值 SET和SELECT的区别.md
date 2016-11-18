title: SQL Server变量赋值 SET和SELECT的区别
id: 742
categories:
  - SqlServer
date: 2013-04-19 09:26:14
tags:
---

SQL Server推荐使用 SET 而不是 SELECT 对变量进行赋值。

**SET**
不支持同时对多个变量同时赋值
表达式返回多个值时出错
表达式未返回值变量被赋null值

**SELECT**
支持同时对多个变量同时赋值
表达式返回多个值时,将返回的最后一个值赋给变量
表达式未返回值,变量保持原值
<!--more-->
例子:

```sql
create table chinadba1(
userid int ,
addr varchar(128)
)
go
insert into chinadba1(userid,addr) values(1,'addr1')
insert into chinadba1(userid,addr) values(2,'addr2')
insert into chinadba1(userid,addr) values(3,'addr3')
go
```

表达式返回多个值时，使用 SET 赋值

```sql
declare @addr varchar(128)
set @addr = (select addr from chinadba1)
go
/*
--报错,子查询返回的值多于一个。
*/
```

表达式返回多个值时，使用 SELECT 赋值

```sql
declare @addr varchar(128)
select @addr = addr from chinadba1
print @addr
--输出: addr3
go
```

表达式未返回值时，使用 SET 赋值

```sql
declare @addr varchar(128)
set @addr = '初始值'
set @addr = (select addr from chinadba1 where userid = 4 )
print @addr
--输出NULL
go
```

表达式未返回值时，使用 SELECT 赋值

```sql
declare @addr varchar(128)
set @addr = '初始值'
select @addr = addr from chinadba1 where userid = 4
print @addr
--输出:初始值
go
```

需要注意的是，SELECT 也可以将标量子查询的值赋给变量，如果标量子查询不返回值，则变量被置为 null 值。
此时与使用 SET 赋值是完全相同的
对标量子查询的概念大家应该都觉得陌生，举个例子就能说明

```sql
declare @addr varchar(128)
set @addr = '初始值'
select @addr = (select addr from chinadba1 where userid = 4)
print @addr
--输出:NULL
go
```

源自:[http://blog.csdn.net/shouyenet1/article/details/3511818](http://blog.csdn.net/shouyenet1/article/details/3511818)
