title: oracle同义词
categories:
  - oracle
date: 2013-03-18 04:54:24
---

赋予user1创建同义词的权限
```sql
grant create any synonym to user1;  --赋予adsend_map创建同义词的权限
revoke create any synonym from user1; --回收adsend_map创建同义词的权限
```

创建同义词例子:
```sql
create public synonym resp_send_cell for adsend_new.resp_send_cell;
```
