title: sqlserver聚合函数列转行
id: 1435
categories:
  - SqlServer
date: 2014-08-09 00:04:11
tags:
---

创建表

```sql
CREATE TABLE T_Score (
	name varchar(10), --姓名
	courseType int,   --学科1:语文;2:数学;3:英语
	score int         --分数
)
```

插入测试数据

```sql
INSERT INTO T_Score (name, courseType, score)
VALUES ('小明', 1, 70)
INSERT INTO T_Score (name, courseType, score)
VALUES ('小明', 2, 75)
INSERT INTO T_Score (name, courseType, score)
VALUES ('小明', 3, 80)
INSERT INTO T_Score (name, courseType, score)
VALUES ('小张', 1, 85)
INSERT INTO T_Score (name, courseType, score)
VALUES ('小张', 2, 90)
INSERT INTO T_Score (name, courseType, score)
VALUES ('小张', 3, 95)
```

查看数据

```sql
SELECT * FROM T_Score
```

name courseType score
小明   1   70
小明   2   75
小明   3   80
小张   1   85
小张   2   90
小张   3   95

通过group by的聚合函数实现列转行

```sql
SELECT name,
	SUM(CASE WHEN courseType = 1 THEN score END) AS '语文',
	SUM(CASE WHEN courseType = 2 THEN score END) AS '数学',
	SUM(CASE WHEN courseType = 3 THEN score END) AS '英语'
  FROM T_Score
 GROUP BY name
```

查询结果

[code lang="txt"]
name     语文     数学     英语
小明     70     75     80
小张     85     90     95
```
