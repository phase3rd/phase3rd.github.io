---
layout: post
title: DB Index 순서에 대하여
published: true
date: 2016-1-14
categories: [sql]
tags: [sql,db,mysql,index,index순서]
---


전에는 항상 WHERE 조건의 순서와 INDEX의 순서를 일치시키도록 했었습니다.
그렇게 까지 할 필요가 없었네요.

INDEX 가 A , B , C 순서로 걸려있는 테이블이 있다고 가정했을때 WHERE 조건의 순서는 상관이 없습니다..

하지만 INDEX 가 걸려있는 순서에 우선적으로 조건을 주어야 합니다.

##### 걸리는 경우는

* A, B, C 3개 모두 포함된 경우

```sql
 EXPLAIN
 SELECT * FROM tb_index_test 
 WHERE ID_C=1 AND ID_B=1 AND ID_A=1;
```

```c
 +----+-------------+---------------+------------+------+---------------+------------+---------+-------------------+------+----------+-------+
 | id | select_type | table         | partitions | type | possible_keys | key        | key_len | ref               | rows | filtered | Extra |
 +----+-------------+---------------+------------+------+---------------+------------+---------+-------------------+------+----------+-------+
 |  1 | SIMPLE      | tb_index_test | NULL       | ref  | TRIPLE_KEY    | TRIPLE_KEY | 15      | const,const,const |    1 |   100.00 | NULL  |
 +----+-------------+---------------+------------+------+---------------+------------+---------+-------------------+------+----------+-------+
 1 row in set, 1 warning (0.00 sec)
```

* A , B 또는 C 가 걸린 경우

```sql
 EXPLAIN
 SELECT * FROM tb_index_test 
 WHERE ID_C=1  AND ID_A=1;
```

```c
 +----+-------------+---------------+------------+------+---------------+------+---------+------+------+----------+-------------+
 | id | select_type | table         | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
 +----+-------------+---------------+------------+------+---------------+------+---------+------+------+----------+-------------+
 |  1 | SIMPLE      | tb_index_test | NULL       | ALL  | TRIPLE_KEY    | NULL | NULL    | NULL |    8 |    12.50 | Using where |
 +----+-------------+---------------+------------+------+---------------+------+---------+------+------+----------+-------------+
 1 row in set, 1 warning (0.00 sec)
```

* A 만 단독으로 걸린경우

```sql
 EXPLAIN
 SELECT * FROM tb_index_test 
 WHERE ID_A=1;
```

```c
 +----+-------------+---------------+------------+------+---------------+------+---------+------+------+----------+-------------+
 | id | select_type | table         | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
 +----+-------------+---------------+------------+------+---------------+------+---------+------+------+----------+-------------+
 |  1 | SIMPLE      | tb_index_test | NULL       | ALL  | TRIPLE_KEY    | NULL | NULL    | NULL |    8 |    50.00 | Using where |
 +----+-------------+---------------+------------+------+---------------+------+---------+------+------+----------+-------------+
 1 row in set, 1 warning (0.00 sec)
```

#### 나머지는 안걸림
