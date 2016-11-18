title: oracle复制表
categories:
  - oracle
date: 2012-09-18 07:11:15
---

方法一(PL-SQL中不可用)
```sql
SELECT vale1, value2 INTO NewTable FROM OldTable;
```

 
方法二
```sql
--复制数据和表结构
CREATE TABLE NewTable AS SELECT * FROM OldTable;

--仅复制表结构
CREATE TABLE NewTable AS SELECT * FROM OldTable WHERE ROWNUM < 0;
```
