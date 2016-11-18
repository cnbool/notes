title: oracle表空间操作
categories:
  - oracle
date: 2013-01-05 21:45:10
---

**临时表空间**
新建临时表空间(自动增长)
```sql
CREATE TEMPORARY TABLESPACE TB_TEMP
TEMPFILE 'D:oracleproduct10.2.0oradataorcltablespace_tempTB_TEMP.dbf' 
SIZE 2048M AUTOEXTEND ON NEXT 1024M MAXSIZE UNLIMITED EXTENT MANAGEMENT LOCAL;
```

查看临时表空间 （dba_temp_files视图）（v_$tempfile视图）
```sql
select tablespace_name,file_name,bytes/1024/1024 file_size,autoextensible from dba_temp_files;
select status,enabled, name, bytes/1024/1024 file_size from v_$tempfile;--sys用户查看
```

缩小临时表空间大小
```sql
alter database tempfile 'D:ORACLEPRODUCT10.2.0ORADATATELEMTTEMP01.DBF' resize 100M;
```

**扩展临时表空间：**
方法一、增大临时文件大小：
```sql
alter database tempfile '/u01/app/oracle/oradata/orcl/temp01.dbf' resize 100m;
```
方法二、将临时数据文件设为自动扩展：
```sql
alter database tempfile '/u01/app/oracle/oradata/orcl/temp01.dbf' autoextend on next 5m maxsize unlimited;
```
方法三、向临时表空间中添加数据文件：
```sql
alter tablespace temp add tempfile '/u01/app/oracle/oradata/orcl/temp02.dbf' size 100m;
```

更改数据库默认临时表空间
```sql
ALTER DATABASE DEFAULT TEMPORARY TABLESPACE TB_TEMP;
```

**数据表空间**
新建数据表空间(自动增长)
```sql
CREATE TABLESPACE TB_DATA 
DATAFILE 'D:oracleproduct10.2.0oradataorcltablespace_dataTB_DATA.dbf' 
SIZE 1024M AUTOEXTEND ON NEXT 1024M MAXSIZE UNLIMITED EXTENT MANAGEMENT LOCAL;
```

添加表空间文件(自动增长)
```sql
ALTER TABLESPACE TB_DATA ADD
DATAFILE 'D:oracleproduct10.2.0oradataorcltablespace_dataTB_DATA.dbf' 
SIZE 1024M AUTOEXTEND ON NEXT 1024M MAXSIZE UNLIMITED;
```

修改表空间大小
```sql
ALTER DATABASE
DATAFILE 'D:oracleproduct10.2.0oradataorcltablespace_dataTB_DATA.dbf' 
RESIZE 200M;
```

删除表空间及其物理文件
```sql
DROP TABLESPACE TB_DATA INCLUDING CONTENTS AND DATAFILES CASCADE CONSTRAINTS;
```

**用户与表空间**
新建用户并指定默认表空间,默认临时表空间
```sql
CREATE USER xiaoming IDENTIFIED BY xiaoming_pwd 
DEFAULT TABLESPACE TB_DATA 
TEMPORARY TABLESPACE TB_TEMP;
```

更改用户临时表空间
```sql
ALTER USER xiaoming TEMPORARY TABLESPACE TB_TEMP;
```

更改用户默认表空间
```sql
ALTER USER xiaoming DEFAULT TABLESPACE TB_DATA;
```

删除用户及其内容
```sql
DROP USER xiaoming CASCADE;
```

赋予用户管理员权限
```sql
GRANT RESOURCE, CONNECT, DBA TO adsend;
```

**表空间数据文件的移动**
1.先将表空间offline

```sql
ALTER TABLESPACE 表空间名字 OFFLINE;
```

2.手工移动数据文件的位置
3.执行命令

```sql
ALTER TABLESPACE 表空间名字 RENAME DATAFILE '数据文件原位置' TO '数据文件更改后位置';
```

4.表空间online

```sql
ALTER TABLESPACE 表空间名字 ONLINE;
```
**
查看Oracle数据库中表空间信息**
```sql
select
a.a1 表空间名称,
c.c2 类型,
c.c3 区管理,
b.b2/1024/1024 表空间大小M,
(b.b2-a.a2)/1024/1024 已使用M,
substr((b.b2-a.a2)/b.b2*100,1,5) 利用率
from
(select  tablespace_name a1, sum(nvl(bytes,0)) a2 from dba_free_space group by tablespace_name) a,
(select tablespace_name b1,sum(bytes) b2 from dba_data_files group by tablespace_name) b,
(select tablespace_name c1,contents c2,extent_management c3  from dba_tablespaces) c
where a.a1=b.b1 and c.c1=b.b1;
```

**查看Oracle数据库中数据文件**
```sql
select
b.file_name 物理文件名,
b.tablespace_name 表空间,
b.bytes/1024/1024 大小M,
(b.bytes-sum(nvl(a.bytes,0)))/1024/1024  已使用M,
substr((b.bytes-sum(nvl(a.bytes,0)))/(b.bytes)*100,1,5)  利用率
from dba_free_space a,dba_data_files b
where a.file_id=b.file_id
group by b.tablespace_name,b.file_name,b.bytes
order by b.tablespace_name
```
