title: sqlserver存储过程:将空字段置为0
id: 1433
categories:
  - SqlServer
date: 2014-08-08 23:38:53
tags:
---

```sql
CREATE PROC P_Field_Null2Zero (
	@tableName varchar(100)
	) 
	AS
	DECLARE fieldName_Cursor CURSOR LOCAL FOR
	SELECT name, xusertype, [status] from SysColumns WHERE id=Object_Id(@tableName)

	OPEN fieldName_Cursor
	DECLARE @fieldName VARCHAR(50)
	DECLARE @fieldType int
	DECLARE @status int

	FETCH NEXT FROM fieldName_Cursor INTO @fieldName, @fieldType, @status
	WHILE @@FETCH_STATUS = 0
	BEGIN
	IF (@fieldType = 56 OR @fieldType = 106) AND @status = 0x08 --status=0x08表示字段可以为NULL
	BEGIN
		--PRINT 'UPDATE ' + @tableName + ' SET ' + @fieldName + ' = 0 WHERE ' + @fieldName + ' IS NULL'
		EXEC ('UPDATE ' + @tableName + ' SET ' + @fieldName + ' = 0 WHERE ' + @fieldName + ' IS NULL')
	END
	FETCH NEXT FROM fieldName_Cursor INTO @fieldName, @fieldType, @status
	END

	CLOSE fieldName_Cursor
	DEALLOCATE fieldName_Cursor
```
