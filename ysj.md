---
title: 数据库学习
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---
### 数据库相关面试题(1)

==数据库的表格如下： #f44336==
| y_year | y_month | y_amount |
| ---- | ----- | ------ |
| 1991 | 1     | 1.1    |
| 1991 | 2     | 1.2    |
| 1991 | 3     | 1.3    |
| 1991 | 4     | 1.4    |
| 1992 | 1     | 2.1    |
| 1992 | 2     | 2.2    |
| 1992 | 3     | 2.3    |
| 1992 | 4     | 2.4     |
==要求写出的sql语句如下==

| y_year | m1  | m2  | m3  |  m4   |
| ---- | --- | --- | --- | --- |
| 1991 | 1.1 | 1.2 | 1.3 | 1.4 |
| 1992 | 2.1 | 2.2 | 2.3 |  2.4   |

==sql语句如下:==
```sql
SELECT e.y_year,
  (SELECT y_amount FROM ysj_data a WHERE a.y_month = 1 AND a.y_year = e.y_year ) AS m1,
  (SELECT y_amount FROM ysj_data b WHERE b.y_month = 2 AND b.y_year = e.y_year ) AS m2,
  (SELECT y_amount FROM ysj_data c WHERE c.y_month = 3 AND c.y_year = e.y_year ) AS m3,
  (SELECT y_amount FROM ysj_data d WHERE d.y_month = 4 AND d.y_year = e.y_year ) AS m4
  FROM ysj_data e GROUP BY e.y_year;
```
==分析：数据库查询之后要求3行9列的表格
利用y_month分组 还有子查询==

### 数据库相关面试题(2)
==数据库的表格如下： #f44336==

| day_id     | result  |
| ---------- | ------- |
| 2018-10-11 | success |
| 2018-10-11 | fail    |
| 2018-10-11 | fail    |
| 2018-10-11 | success |
| 2018-10-12 | success |
| 2018-10-12 | fail    |
| 2018-10-12 | fail      |
==要求写出的sql语句如下==

| day_id     | success | fail |
| ---------- | ------- | ---- |
| 2018-10-11 | 2       | 2    |
| 2018-10-12 | 1       | 2     |

==sql语句如下==
```sql
SELECT
a.`day_id`,
(SELECT COUNT(*) FROM ysj_test b WHERE b.result ='success' AND a.day_id = b.day_id) AS success,
(SELECT COUNT(*) FROM ysj_test c WHERE c.result = 'fail' AND a.`day_id` = c.day_id) AS fail
FROM ysj_test a GROUP BY a.day_id;
```
==分析还是要的用到子查询把子查询在selec后作为查询的条件==

### 数据库相关面试题(3)

==数据库的表格数据如下: #f44336==

| id  | movie   | desctiption | rating |
| --- | ------- | ----------- | ------ |
| 1   | War     | Intersting  | 9.8    |
| 2   | Science | funning     | 7.8    |
| 3   | Love    | boring      | 3.6    |
| 4   | KongFu  | fantcy      | 8.8    |
| 5   |         | great       | 9.9    |

==要求查询到id为奇数并且description是boring的电影
注意：假设数据库中的数据库很多==
```sql
SELECT * FROM cinema WHERE MOD(id,2)=1
AND cinema.`description`='boring';
```
分析：
==select mod(id,2)的结果是1,那么id是结束 #e91e63==

### 数据库面试题(4)
==数据库中数据如下： #f44336==

| id  | email       |
| --- | ----------- |
| 1   | ysj@qq.com  |
| 2   | ysj@163.com |
| 3   |  ysj@qq.com           |
要求查询重复的email
```sql
SELECT email,COUNT(email) FROM school GROUP BY email HAVING COUNT(email)>1;
```
==要求删除相同的数据库的email并且保留id最小的email:==

| id  | email      |
| --- | ---------- |
| 1   | ysj@qq.com |
| 2   | ysj@163.com           |

```sql
DELETE s1
FROM school s1,school s2
WHERE s1.`email` = s2.`email`
AND s1.`id`>s2.`id`;


SELECT * FROM school ORDER BY school.`id`;
```
### 数据库面试题(5)
==数据库中的数据如下： #f44336==

| id  | score |
| --- | ----- |
| 1   | 3.50  |
| 2   | 3.65  |
| 3   | 4.00  |
| 4   | 3.85  |
| 5   | 4.00  |
| 6   | 3.65      |
==要查询到数据库如下表显示:==

| score | rank |
| ----- | ---- |
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4   |

要求按照成绩排名并且相同分数显示同样排名
```sql
SELECT 
  g.score,
  (SELECT 
    COUNT(DISTINCT grade.`score`) 
  FROM
    grade 
  WHERE grade.`score` >= g.`score`) AS rank 
FROM
  grade g 
ORDER BY g.score DESC ;
```
**==分析每次拿成绩比较并且相同成绩也做比较，把重复的成绩给剔除 #f44336==**


### 数据库面试题(6)
==给定数据结构如下表: #f44336==
==部门表: #4caf50==

| id  | name |
| --- | ---- |
| 1   | IT   |
| 2   |  Sales    |

==员工信息表:==

| id  | name   | salary | departmentId |
| --- | ------ | ------ | ------------ |
| 1   | Joe    | 70000  |     1         |
| 2   | Henery | 80000  |      2        |
| 3   | Max    | 90000  |        2      |
| 4   | Jim    |    60000    |      1        |

==要求查询每个部门的最高工资：==

|     |     |
| --- | --- |
|     |     |
|     |     |
|     |     |
|     |     |
|     |     |
|     |     |
|     |     |







