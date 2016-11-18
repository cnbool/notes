title: mysql存储过程与sqlserver区别
categories:
  - Mysql
  - SqlServer
date: 2016-01-19 17:39:10
tags:
---

## 1.参数 ##
mysql存储过程的参数IN/OUT/INOUT等参数在前,sqlserver传出参数在末尾加out
sqlserver存储过程参数名以@开头，mysql参数名无@

## 2.调用 ##
sqlserver存储过程可以使用 EXEC和CALL方式调用
sqlserver调用存储过程时，传出参数需要加out关键字，例如：

```sql
DECLARE @param1 VARCHAR(10)
DECLARE @param2 VARCHAR(10)
SET @param1 = 'XIAOMING'
EXEC C_MyProc @param1, @param2 out

PRINT @param2
```

mysql使用CALL调用存储过程：

```sql
SET @p_in=1; 
SET @p_out=2;
SET @p_inout = 's3';
CALL C_MyProc(@p_in,@p_out,@p_inout);
```
## 3.条件判断 ##
** mysql IF THEN条件判断: **

```sql
IF 条件表达式
THEN
	语句
ELSEIF 条件表达式
THEN
	语句
ELSE
	语句
END IF;
```

** sqlserver条件判断： **

```sql
IF 条件表达式
BEGIN
	语句
END
ELSE IF 
BEGIN
	语句
END
ELSE
BEGIN
	语句
END
```