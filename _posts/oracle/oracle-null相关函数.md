title: oracle null相关函数
categories:
  - oracle
date: 2013-01-23 23:05:55
tags:
---

NVL(N, M) N为null的情况下，取M的值，否则取N的值
NVL2(N, A, B) N为null时，取A的值，否则取B的值 (纠正，N为null时，取B的值，否则取A的值)
NULLIF(M, N) 如果 M 和 N 相等，返回 NULL，否则返回 M。
COALESCE(A1, ……,AN ) 返回第一个不为NULL的值。

源自:[http://akunamotata.iteye.com/blog/421730](http://akunamotata.iteye.com/blog/421730 "http://akunamotata.iteye.com/blog/421730")
