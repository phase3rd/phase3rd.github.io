---
layout: post
title: MySQL UNIX_TIMESTAMP 2038 YEAR BUG
published: true
date: 2016-1-11
categories: [sql]
tags: [db,sql,mysql]
---

#### MySQL 의 UNIX_TIMESTAMP 함수는 2038년 이후는 작동하지 않습니다.

최근 UNIX_TIMESTAMP를 사용하면서 알게된 사실
아직도(v5.7) 32bit 를 사용하기 때문에 2038년이 계산이 되지 않습니다.

```sql
 SELECT unix_timestamp('2038-1-31 00:00:00')
```
> 결과
```c
+--------------------------------------+
| unix_timestamp('2038-1-31 00:00:00') |
+--------------------------------------+
|                                    0 |
+--------------------------------------+
```

버그를 피하기위해 직접 계산하는 함수를 만드는것도 방법입니다.

```sql
 SELECT (((TO_DAYS(date) * 86400) + TIME_TO_SEC(date)) - (TO_DAYS("1970-01-01") * 86400));
```
