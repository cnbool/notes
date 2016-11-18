title: oracle设置账户密码永不过期
categories:
  - oracle
date: 2013-12-24 01:43:13
tags:
---

1.查看用户的proifle
```sql
SELECT USERNAME,PROFILE FROM DBA_USERS;
```

2.查看proifle密码有效期设置
```sql
SELECT * FROM DBA_PROFILES WHERE PROFILE='DEFAULT' AND RESOURCE_NAME='PASSWORD_LIFE_TIME';
```

3.密码有效期改为无限
```sql
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```

如果账户提示锁定
```sql
ALTER USER XIAOMING ACCOUNT UNLOCK;
```

如果账户仍然提示过期,重置账户密码
```sql
ALTER USER XIAOMING IDENTIFIED BY PWD12345;
```
