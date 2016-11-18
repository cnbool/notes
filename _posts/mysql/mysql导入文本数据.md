title: mysql导入文本数据
id: 1262
categories:
  - Mysql
date: 2014-03-06 02:28:10
tags:
---

```sql
LOAD DATA [LOW_PRIORITY | CONCURRENT] [LOCAL] INFILE 'file_name.txt'
    [REPLACE | IGNORE]
    INTO TABLE tbl_name
    [FIELDS
        [TERMINATED BY 'string']
        [[OPTIONALLY] ENCLOSED BY 'char']
        [ESCAPED BY 'char' ]
    ]
    [LINES
        [STARTING BY 'string']
        [TERMINATED BY 'string']
    ]
    [IGNORE number LINES]
    [(col_name_or_user_var,...)]
    [SET col_name = expr,...)]
```

例:
```sql
load data local infile 'e:/2014-02-new-users.txt'
into table temp_data
FIELDS TERMINATED BY ',';
```
