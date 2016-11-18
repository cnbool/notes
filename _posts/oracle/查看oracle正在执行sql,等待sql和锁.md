title: 查看oracle正在执行sql,等待sql和锁
categories:
  - oracle
date: 2013-01-26 01:37:03
---

查询当前执行的SQL SID, CPU_TIME， SQL_TEXT
```sql
select SID,SERIAL#, OSUSER, PROGRAM, USERNAME, SCHEMANAME, B.Cpu_Time, STATUS, B.SQL_TEXT
from V$SESSION A
LEFT JOIN V$SQL B ON A.SQL_ADDRESS = B.ADDRESS
AND A.SQL_HASH_VALUE = B.HASH_VALUE
WHERE SID IN (SELECT SID FROM v$session_wait)
order by b.cpu_time desc;

kill session:
alter system kill session 'sid, s.serial#';
alter system kill session '117 ,2929';
```

确定等待最多的SQL
```sql
SELECT a.user_id, d.username, s.sql_text,
SUM(a.wait_time + a.time_waited) total_wait_time
FROM v$active_session_history a,
v$sqlarea s,
dba_users d
WHERE a.sample_time between sysdate - 30/2800 and sysdate
AND a.sql_id = s.sql_id
AND a.user_id = d.user_id
GROUP BY a.user_id, s.sql_text, d.username
ORDER BY total_wait_time DESC;
```

v$system_wait_class
```sql
SELECT wait_class, time_waited
FROM v$system_wait_class
ORDER BY time_waited DESC;
```

按照单个会话得到每种等待花费时间
```sql
SELECT wait_class, time_waited
FROM v$session_wait_class
WHERE sid = 117
ORDER BY time_waited DESC;
```

查出oracle当前的被锁对象
```sql
SELECT l.session_id sid,
       s.serial#,
       l.locked_mode 锁模式,
       l.oracle_username 登录用户,
       l.os_user_name 登录机器用户名,
       s.machine 机器名,
       s.terminal 终端用户名,
       o.object_name 被锁对象名,
       s.logon_time 登录数据库时间
FROM v$locked_object l, all_objects o, v$session s
WHERE l.object_id = o.object_id
   AND l.session_id = s.sid
ORDER BY sid, s.serial#;
```

查看正在执行sql的发起者的程序
```sql
SELECT OSUSER 电脑登录身份,
       PROGRAM 发起请求的程序,
       USERNAME 登录系统的用户名,
       SCHEMANAME,
       B.Cpu_Time 花费cpu的时间,
       STATUS,
       B.SQL_TEXT 执行的sql
FROM V$SESSION A
LEFT JOIN V$SQL B ON A.SQL_ADDRESS = B.ADDRESS
                   AND A.SQL_HASH_VALUE = B.HASH_VALUE
ORDER BY b.cpu_time DESC
```

查询oracle job信息
```sql
 select job_name,comments from dba_scheduler_jobs;
```

disable automatic optimizer statistics collection
```sql
BEGIN
  DBMS_AUTO_TASK_ADMIN.DISABLE(
    client_name => 'auto optimizer stats collection', 
    operation => NULL, 
    window_name => NULL);
END;
```

enable automatic optimizer statistics collection
```sql
BEGIN
  DBMS_AUTO_TASK_ADMIN.ENABLE(
    client_name => 'auto optimizer stats collection', 
    operation => NULL, 
    window_name => NULL);
END;
```
