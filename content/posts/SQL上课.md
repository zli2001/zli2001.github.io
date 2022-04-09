---
title: "SQL上课"
date: 2022-04-01T19:04:06+08:00
draft: true
tags: ["code","STL"]
series: ["SQL上课"]
categories: ["Coding"]
---
# 上课
## 游标
1. 声明
```sql
DECLARE StuCur SCROLL CURSOR --创建一个游标
-- SCROLL 动态游标，可前进可后退
-- 不写SCROLL 静态游标，只能一步一步前进，不能后退
FOR SELECT * FROM student
FOR UPDATE OF email
--锁字段 也就是从你select语句执行后 表的email 字段被锁定 别人将不能进行修改操作 直到你进行update commit后才能修改
DECLARE @学号 char(10) @姓名char(10)@...--定义一些变量，用来导出游标所指数据
--@xx 变量 @@xx 系统变量
```
2. 打开游标

```sql
OPEN stucur
IF @@ERROR=0 --若成功打开一个游标
   PRINT '共有'+ CONVERT(VARCHAR(3),@@cursor_rows)+'个学生'
-- @@cursor_rows 游标所指表的总行数
-- 初始游标指向表的顶端，需要下移一行才有数据
FETCH NEXT FROM stucur INTO @学号,@姓名,@...--导出游标的数据
--有多少属性就导出多少
PRINT @...
```
3. 修改
```sql
FETCH NEXT FROM stucur --静态只能用next
--动态可以用PRIOR ,NEXT ,FIRST ,LAST ,ABSOLUTE ,RELATIVELY
UPDATE student
SET email='..' WHERE CURRENT OF StuCur
```
4. 关闭释放游标
```sql
CLOSE cursor
DEALLOCATE stucur
```
