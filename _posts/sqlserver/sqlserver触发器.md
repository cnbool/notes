title: sqlserver触发器
id: 924
categories:
  - SqlServer
date: 2013-07-03 21:28:29
tags:
---

**Instead of 和 After触发器**
SQL Server2000提供了两种触发器:Instead of 和After 触发器。
Instead of触发器用于替代引起触发器执行的T-SQL语句。除表之外,Instead of 触发器也可以用于视图,用来扩展视图可以支持的更新操作。
After触发器在一个Insert,Update或Deleted语句之后执行,进行约束检查等动作都在After触发器被激活之前发生。After触发器只能用于表。
一个表或视图的每一个修改动作(insert,update和delete)都可以有一个instead of 触发器,一个表的每个修改动作都可以有多个After触发器。
例子:
```sql
create trigger user_info_insert
on user_info
after insert
as
insert into user_log(phone_num, oper_type, oper_way, oper_time) 
select phone_num, 0, 4, getdate() from inserted

create trigger user_info_update
on user_info
after update
as
insert into user_log(phone_num, oper_type, oper_way, oper_time) 
select phone_num, 1, 4, getdate() from deleted

create trigger user_info_delete
on user_info
after delete
as
insert into user_log(phone_num, oper_type, oper_way, oper_time) 
select phone_num, 2, 4, getdate() from deleted
```
部分内容源自:[http://www.cnblogs.com/gsk99/archive/2011/08/31/2160973.html](http://www.cnblogs.com/gsk99/archive/2011/08/31/2160973.html)
