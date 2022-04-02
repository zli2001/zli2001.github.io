---
title: "SQL必知必会"
date: 2022-03-25T19:45:02+08:00
draft: false
tags: ["code","STL"]
series: ["SQL必知必会"]
categories: ["Coding"]
---

<!--more-->

## Lesson 5 高级数据过滤
```sql

--operator AND IN OR

SELECT prod_id,prod_price,prod_name
FROM Products
WHERE vend_id ='DLL01' AND prod_price <= 4;

SELECT prod_id,prod_price,prod_name,vend_id
FROM Products
WHERE vend_id ='DLL01' OR vend_id ='BRS01';
--AND 比OR优先级高 
SELECT prod_price,prod_name
FROM Products
WHERE (vend_id ='DLL01' OR vend_id = 'BRS01')
		AND prod_price >= 10;


--IN 与or功能相同
--比OR快
SELECT prod_id,prod_price,prod_name,vend_id
FROM Products
WHERE vend_id IN ('DLL01','BRS01');

--NOT 否定条件,与其他条件一起使用

SELECT prod_name,vend_id
FROM Products
WHERE NOT vend_id IN ('DLL01','BRS01');

--5.5.1
SELECT vend_name,vend_state,vend_country
FROM Vendors
WHERE vend_country='USA' AND vend_state='CA';

--5.5.2
SELECT order_num,prod_id,quantity
FROM OrderItems
WHERE prod_id IN ('BR01','BR02','BR03') AND quantity>=100;

--5.5.3
SELECT prod_name,prod_price
FROM Products
WHERE prod_price>3 AND prod_price<6
ORDER BY prod_price
--
```
## Lesson 6 用通配符进行过滤

```sql
--通配符只能用于文本字段

SELECT prod_id,prod_name
FROM Products
WHERE prod_name LIKE 'Fish%';--%表示任意字符出现任意次数，Fish开头的

SELECT prod_id,prod_name
FROM Products
WHERE prod_name LIKE 'F%y';-- WHERE email LIKE 'b%@forta.com'


SELECT prod_id,prod_name
FROM Products
WHERE prod_name LIKE '_ inch teddy bear';
-- _ 下划线只匹配单个字符
--输出 8 inch teddy bear, 而不会返回 12 inch teddy bear


SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[JM]%'--[]匹配方括号中任意一个字符，%匹配第一个字符后的任意数目字符。返回以J或M开头的
ORDER BY cust_contact;


SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[^JM]%'-- ^脱字号 否定
ORDER BY cust_contact;

SELECT cust_contact
FROM Customers
WHERE NOT cust_contact LIKE '[JM]%'-- NOT 否定
ORDER BY cust_contact;

--6.4.1
SELECT prod_desc,prod_name
FROM Products
WHERE prod_desc LIKE '%toy%';

--6.4.2
SELECT prod_desc,prod_name
FROM Products
WHERE NOT prod_desc LIKE '%toy%';


--6.4.3
SELECT prod_desc,prod_name
FROM Products
WHERE (prod_desc LIKE '%toy%' )
AND (prod_desc LIKE '%carrots%');

--6.4.4
SELECT prod_desc,prod_name
FROM Products
WHERE prod_desc LIKE '%toy%carrots%' ;

```

## Lesson 7 创建计算字段
```sql

--拼接 +
SELECT vend_name + '('+ vend_country + ')'
FROM Vendors
ORDER BY vend_name;

--去掉值右边的全部空格 ：RTRIM()函数
--去掉左边的空格LTRIM(),TRIM()去掉左右的空格
SELECT RTRIM(vend_name)+'('+RTRIM(vend_country)+')'
FROM Vendors
ORDER BY vend_name;

--别名 as
SELECT RTRIM(vend_name)+'('+RTRIM(vend_country)+')'
AS vend_title
FROM Vendors
ORDER BY vend_name;

--执行算数计算
SELECT prod_id,quantity,item_price,
quantity*item_price AS expand_price--计算字段
FROM OrderItems
WHERE order_num=20008;

--7.5.1
SELECT  vend_id,vend_name AS vname,
vend_address AS vaddress,
vend_city AS vcity
FROM Vendors
ORDER BY vend_name;

--7.5.2
SELECT prod_id,prod_price,
prod_price*0.9 AS sale_price--计算字段
FROM Products;
```