title: SqlServer生成特定区间的时间变量
id: 1708
comment: false
categories:
  - SqlServer
date: 2015-07-06 18:16:28
tags:
---
		查询区间前一个小时
		```sql
    DECLARE @start DATETIME
    DECLARE @end DATETIME

    SET @end = CONVERT(DATETIME, CONVERT(VARCHAR(14), GETDATE(), 120) + '00:00')
    SET @start = DATEADD(HOUR, -1, @end)
    ```
    
		昨日凌晨0点到今日凌晨0点
		```sql
		DECLARE @beginTime DATETIME
		DECLARE @endTime DATETIME
		
		SET @endTime = DATEADD(DAY, 0, DATEDIFF(DAY, 0, GETDATE()))
		SET @beginTime = DATEADD(DAY, -1, @endTime)
		
		SELECT @beginTime, @endTime
		```