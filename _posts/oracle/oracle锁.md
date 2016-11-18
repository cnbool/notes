title: oracle锁
categories:
  - oracle
date: 2013-01-26 01:25:38
---

```sql
select * from v$lock;

select * from v$locked_object;

select b.owner,b.object_name,a.session_id,a.locked_mode
from v$locked_object a,dba_objects b
where b.object_id = a.object_id;

select b.owner,b.object_name,l.session_id,l.locked_mode
from v$locked_object l, dba_objects b
where b.object_id=l.object_id

select t2.username,t2.sid,t2.serial#,t2.logon_time 
from v$locked_object t1,v$session t2 
where t1.session_id=t2.sid order by t2.logon_time
```

ORACLE里锁有以下几种模式:

0：none

1：null 空

2：Row-S 行共享(RS)：共享表锁，sub share

3：Row-X 行独占(RX)：用于行的修改，sub exclusive

4：Share 共享锁(S)：阻止其他DML操作，share

5：S/Row-X 共享行独占(SRX)：阻止其他事务操作，share/sub exclusive

6：exclusive 独占(X)：独立访问使用，exclusive

数字越大锁级别越高, 影响的操作越多。

1级锁有：Select，有时会在v$locked_object出现。

2级锁有：Select for update,Lock For Update,Lock Row Share

select for update当对话使用for update子串打开一个游标时，所有返回集中的数据行都将处于行级(Row-X)独占式锁定，其他对象只能查询这些数据行，不能进行update、delete或select for update操作。

3级锁有：Insert, Update, Delete, Lock Row Exclusive

没有commit之前插入同样的一条记录会没有反应, 因为后一个3的锁会一直等待上一个3的锁, 我们必须释放掉上一个才能继续工作。

4级锁有：Create Index, Lock Share

locked_mode为2,3,4不影响DML(insert,delete,update,select)操作, 但DDL(alter,drop等)操作会提示ora-00054错误。

00054, 00000, "resource busy and acquire with NOWAIT specified"

// *Cause: Resource interested is busy.

// *Action: Retry if necessary.

5级锁有：Lock Share Row Exclusive

具体来讲有主外键约束时update / delete ... ; 可能会产生4,5的锁。

6级锁有：Alter table, Drop table, Drop Index, Truncate table, Lock Exclusive

 

内容源自:[http://www.cnblogs.com/elucsn/archive/2012/04/05/2433315.html](http://www.cnblogs.com/elucsn/archive/2012/04/05/2433315.html "http://www.cnblogs.com/elucsn/archive/2012/04/05/2433315.html")
