title: sqlserver数据库备份恢复到其他机器后无法在当前安全上下文下访问数据库问题
id: 1478
categories:
  - SqlServer
date: 2014-09-27 22:52:56
tags:
---

```sql
ALTER DATABASE 数据库名 SET TRUSTWORTHY ON
ALTER AUTHORIZATION ON DATABASE::数据库名 TO 用户名
```
