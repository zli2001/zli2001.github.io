---
title: "SQL上课"
date: 2022-04-01T19:04:06+08:00
draft: false

lastmod: 2022-04-27T19:04:06+08:00
draft: false
author: "kliiu"
authorLink: "https://kliiu.github.io"
description: "Notes of STL"
tags: ["code","STL"]
categories: ["Coding","STL"]

lightgallery: true
---
SQL上课笔记
<!--more-->

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

## 创建表

从已有的表创建表

1. 只要表结构不要数据 :
2. 既要表结构又要数据 :

```sql
--1
select * into newtable from oldtable where 1=2
--2
select * into newtable from oldtable
```
## 存储过程
1. 无参数
    ```sql
    --利用存储过程，查询所有学生信息
    USE teaching
    GO
    IF EXISTS(
        SELECT * FROM SYSOBJECTS
            WHERE name='pro1' AND type='p')
        DROP PROCEDURE pro1
        GO
    CREATE PROCEDURE pro1
    AS SELECT * FROM student
    GO
    EXEC pro1;
    ```
    
2. 带输入参数的存储过程

   例：创建一个查找某同学信息的存储过程
    ```sql
    --1.打开DB
    USE teaching
    GO
    --2.查重
    IF EXISTS(
        SELECT * FROM SYSOBJECTS
            WHERE name='pro2' AND type='P')--P必须大写
        DROP PROCEDURE pro2
        GO
    --3.创建
    CREATE PROCEDURE pro2 @stuname char(10)
    --输入参数类型与表中一致:char(10)
    AS SELECT * FROM student
    WHERE sname LIKE @stuname
    GO
    --4.调用
    EXEC pro2 'Alice';
    -- or:
    --EXEC pro2 @stuname='Alice';
   
    ```
   
3. 带输入输出
	eg: 查找某同学学号和tel
	
	```sql
	--1.打开DB
	 USE teaching
	 GO
	 --2.查重
	 IF EXISTS(
	     SELECT * FROM SYSOBJECTS
	         WHERE name='pro3' AND type='P')--P必须大写
	     DROP PROCEDURE pro3
	     GO
	 --3.创建
	 CREATE PROCEDURE pro3 @stuname nchar(8),@stuid nchar(10) OUTPUT, 	@stutel char(12) OUTPUT
	 AS 
	 SELECT @stuid=studentno,@stutel=phone --字段名
	 FROM student
	 WHERE sname = @stuname
	 GO
	
	 --4.调用
	 DECLARE @sn nchar(10),@st char(12)
	 EXEC pro3 '梁欣',@SN OUTPUT,@ST OUTPUT
	 PRINT '梁欣的学号和电话分别为：'+@sn+','+@st;
	```
	
4. 使用return返回pro的值，只能是整数

    1. 使用pro向学生表插入一行记录
    
       ```sql
       --1.
       USE teaching
       GO 
       --2.
         IF EXISTS(
            SELECT * FROM SYSOBJECTS
                WHERE name='pro4' AND type='P')--P必须大写
            DROP PROCEDURE pro4
            GO
       --3.
       CREATE PROCEDURE pro4 @stuid nchar(12),
       					@stuname nchar(8),@stusex 
       nchar(1),@stuemail nchar(20)--...与原表一致
       					,@flag int OUTPUT
       AS
       	BEGIN --代码过长时用begin and 括起来
       		SET @flag=1
	   		IF EXISTS (SELECT *
	   				  FROM student
	                     WHERE studentno=@stuid)
	              SET @flag=0
	           ELSE--end if
	           	INSERT INTO student(studentno,
	                                   sname,
                                       sex,
                                      	email)
                   VALUES(@stuid,@stuname,@stusex,@stuemail)
           END   --end begin    
       --4.调用
       DECLARE @flag int
       EXEC pro4 '2020001','Alice','女','alice@163.com',--...
       		  @flag OUTPUT
       IF @flag=0 
       	PRINT '已存在学生，无法插入'
       ELSE 
       	PRINT '插入成功'
       ```
    
       
    
    2. 向学生信息表删除一行记录
    
       ```sql
       --1.
       USE teaching
       GO 
       --2.
         IF EXISTS(
            SELECT * FROM SYSOBJECTS
                WHERE name='pro5' AND type='P')--P必须大写
            DROP PROCEDURE pro5
            GO
       --3.创建
       CREATE PROCEDURE pro5 @stuid nchar(12),
       					@flag int OUTPUT
       AS
       	BEGIN --代码过长时用begin and 括起来
       		SET @flag=1
       		IF EXISTS (SELECT *
       				  FROM score
                         WHERE studentno=@stuid)
                  --有选课记录不能删除
                  SET @flag=0
               ELSE--end if
               	DELETE FROM student
               	WHERE studentno=@stuid
           END   --end begin    
       RETURN    
       --4.调用
       DECLARE @flag int
       EXEC pro5 '2020001',
       		  @flag OUTPUT
       IF @flag=0 
       	PRINT '该生有选课，无法删除'
       ELSE 
       	PRINT '删除成功'
       ```


## 函数

1. 标量函数

```sql
-- 函数
--1

IF EXISTS(SELECT * 
		  FROM SYSOBJECTS
		  WHERE NAME='fun1' AND TYPE='FN'
		  )
	DROP FUNCTION func1
GO
CREATE FUNCTION dbo.func1(@stuSex nchar(1)) --函数的参数有括号
RETURNS int
AS 
BEGIN
	DECLARE @num int
	SET @num=0
	SELECT @num=COUNT(*) FROM student
	WHERE sex=@stuSex
	RETURN @num --区别3：return在begin end里
END
GO
--调用
DECLARE @num int
SET @num=dbo.func1('男') -- 函数直接赋值调用，无需exec
PRINT('男生人数为：'+cast(@num AS char(2)))
GO
```

2. 内嵌表值函数

   ```SQL
   --2.
   -- 输出某同学的学号、姓名、成绩
   IF EXISTS(SELECT * 
   		  FROM SYSOBJECTS
   		  WHERE NAME='func2' AND TYPE='IF'
   		  )
   	DROP FUNCTION func2
   GO
   
   CREATE FUNCTION dbo.func2(@stuName AS nchar(8)) --函数的参数有括号
   RETURNS TABLE
   AS 
   RETURN (
   	SELECT student.studentno,sname,courseno,final
   	FROM student,score
   	WHERE student.studentno=score.studentno
   		AND student.sname LIKE @stuName 
   )
   GO
   
   --调用
   SELECT * 
   FROM dbo.func2('韩吟秋')
   GO
   
   ```

   

3. 多语句表值函数

   ```sql
   
   --3.
   --函数前不能用use
   IF EXISTS 
   	(SELECT * 
   	FROM SYSOBJECTS
   	WHERE NAME='func3' AND TYPE='TF'
   	)
   	DROP FUNCTION func3
   GO
   
   --写函数统计某性别人数
   CREATE FUNCTION dbo.func3(@stuNAME AS nchar(8)) 
   RETURNS @stuTable table (学号 nchar(10),
   						 课程号 nchar(10),
   						 成绩 numeric(6,2))
   AS 
   BEGIN
   	INSERT @stuTable --将select结果放入新表
   	SELECT studentno,courseno,final
   	FROM score
   	WHERE studentno IN (SELECT studentno
   						FROM student
   						WHERE sname LIKE @stuName)
   RETURN 
   END
   GO
   
   --调用
   SELECT * 
   FROM dbo.func3('韩吟秋')
   GO
   ```

## 事务
```sql
BEGIN TRANSACTION tran_scode
UPDATE SC SET S#=‘201404001’ WHERE S#=‘201403001’ 
    IF @ERROR<>0
```
## 数据库安全控制
* 授权用户
* 非授权用户

### 授权管理*
GRANT SELECT,INSERT,UPDATE 
ON S
TO xia —-授权给用户xia
## 授权回收*
REVOKE INSERT, UPDATE
ON S
FROM Zia

### 域完整性约束
GRADE CHECK

### 实体完整性
主键（自动）

### 参照完整性
