# SQL必知必会


<!--more-->

## TODO

- [ ] Lesson 13 创建高级联结
- [ ] Lesson 14 组合查询
- [ ] Lesson 16 更新和删除数据
- [ ] Lesson 17 创建和操纵表

## Lesson 2 检索数据

```sql
-- SELECT TOP 2 prod_name 
-- FROM Products
-- 注释
/*多
  行
  注
  释
  
*/
SELECT prod_name 
FROM Products
LIMIT 5 OFFSET 5;
/*
LIMIT 5 OFFSET 5 指示 MySQL等DBMS返回从第 5行起的 5行数据。 第一个数字是检索的行数，第二个数字是指从哪儿开始。
第一个被检索的行是第 0行，而不是第 1行。因此，LIMIT 1 OFFSET 1 会检索第 2行，而不是第 1行。
*/


--2.9.1
--SELECT cust_id
--FROM Customers;

--2.9.2
--DISTINCT 作用于所有列，不仅仅是跟在其后的那一列
--SELECT DISTINCT prod_id
--FROM OrderItems;

--2.9.3
SELECT cust_id
FROM Customers;
```

## Lesson 3 排序检索数据

```sql
SELECT prod_name
FROM Products
ORDER BY prod_name DESC;--（descending）DESC关键字只应用到直接位于其前面的列名

--3.6.1
SELECT cust_name
FROM Customers 
ORDER BY cust_name DESC;

--3.6.2
SELECT cust_id, order_num
FROM Orders
ORDER BY cust_id,order_date DESC;

--3.6.3
SELECT item_price,order_num
FROM OrderItems
ORDER BY order_num,item_price;


```

## Lesson 4 过滤数据

<>：不等于

!<:不小于

BETWEEN 指定的两个值之间

```sql
SELECT prod_id, prod_name, prod_price
FROM Products
WHERE prod_price = 3.49
ORDER BY prod_name; --ORDER BY 放在 WHERE之后

SELECT prod_name,prod_price
FROM Products
WHERE prod_price < 10;

SELECT vend_id,prod_name
FROM Products
WHERE vend_id != 'DLL01';-- <>可以和!=互换

SELECT prod_name,prod_price
FROM Products
WHERE prod_price BETWEEN 5 AND 10;

SELECT cust_name
FROM Customers
WHERE cust_email IS NULL;


--4.4.1
SELECT prod_id,prod_name
FROM Products
WHERE prod_price=9.49;
--4.4.2
SELECT prod_id,prod_name
FROM Products
WHERE prod_price>=9;

--4.4.3
SELECT DISTINCT order_num
FROM OrderItems
WHERE order_num>=100;

--4.4.4
SELECT prod_name,prod_price
FROM Products
WHERE prod_price BETWEEN 3 AND 6
ORDER BY prod_price;
```



## Lesson 5 高级数据 过滤

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
WHERE  vend_id NOT IN ('DLL01','BRS01');

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

--拼接 + 或 ||
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
##  Lesson 8 使用函数处理数据

```sql
--8 使用函数处理数据

--upper()upper,Upper()  转换为大写
--LOWER()			转换为小写
--LEFT()			返回最左边的字符
--RIGHT()			返回最右边的字符
--LTRIN()			去掉左边空格
--RTRIN()			去掉左边空格
--SUBSTR(),SUBSYRING()提取组成部分
--SOUNDEX()   语音类似的字符串
SELECT vend_name,UPPER(vend_name) AS vend_name_upcase
FROM Vendors
ORDER BY vend_name;

--DATEPART()
--返回日期的某一部分
SELECT *
FROM Orders
WHERE DATEPART(yy,order_date) = 2020;

--DATETIME BETWEEN AND
SELECT *
FROM Orders
WHERE order_date BETWEEN '2020-01-01'AND '2021-01-12' ;

--数值处理函数
--ABS()
--COS()
--EXP()
--PI()
--SIN()
--SQRT()
--TAN()

--8.4.1
--SUBSTRING(str,i,len)i表示第i个字符开始（计数从1开始不是从0开始）
--len表示截取长度
SELECT cust_id,cust_name,cust_contact,cust_city,
(SUBSTRING(cust_contact,1,2)+SUBSTRING(cust_city,1,3)) AS user_login
FROM Customers;

--8.4.2
--选择某个时间段的两种方法
SELECT order_num,order_date
FROM Orders
WHERE order_date BETWEEN '2020-01-01'AND '2020-01-31';

SELECT order_num,order_date
FROM Orders
WHERE DATEPART( YY,order_date)=2020 AND DATEPART(MM,order_date)=1;
```

## Lesson 9  汇总数据
```sql
--Lesson 9
--汇总数据

--AVG()
SELECT AVG(prod_price) AS avg_price--只能用于单个列,自动忽略NULL的行
FROM Products;

--COUNT(*)  计算表中行数，不忽略NULL
--COUNT(column)  计算某一列中有值的行数，忽略NULL

--MAX() 忽略NULL
--MIN() 忽略NULL
--SUM()


-- 对以上五个函数，可以指定：
-- ALL(默认)对所有行计算,DISTINCT只包含不同的值

--DISTINCT 不能用于COUNT(*),且必须使用列名，不能用于计算或表达式
SELECT AVG(DISTINCT prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';

--9.5.1
SELECT SUM(quantity)AS sold_prod
FROM OrderItems;

--9.5.2
SELECT SUM(quantity)AS sold_prod
FROM OrderItems
WHERE prod_id ='BR01';

--9.5.3
SELECT MAX(prod_price)AS max_price
FROM Products
WHERE prod_price<10;

```
## Lesson 10 分组数据
```sql
--Lesson10 分组数据
--过滤分组HAVING
--过滤行WHERE
--the same usage 

SELECT cust_id, COUNT(*) AS orders 
FROM Orders 
GROUP BY cust_id
HAVING COUNT(*) >= 2;

--WHERE 在数据分组前进行过滤
--HAVING 在数据分组后进行过滤

SELECT vend_id, COUNT(*) AS num_prods
FROM Products
WHERE prod_price >= 4
GROUP BY vend_id
HAVING COUNT(*) >=2--（计数大于等于2的分组）具有两个以上产品的供应商
ORDER BY vend_id;

--记住语句顺序↑

--10.7.1
SELECT order_num,COUNT(*) AS order_lines
FROM OrderItems
GROUP BY order_num
ORDER BY order_lines;

--10.7.2
SELECT MIN(prod_price) AS chengben
FROM Products
GROUP BY vend_id
ORDER BY chengben;

--10.7.3
--至少含100项的所有订单的订单号
SELECT order_num
FROM OrderItems
GROUP BY order_num
HAVING SUM(quantity)>=100
ORDER BY order_num;

--10.7.4
SELECT order_num,SUM(item_price*quantity) AS total
FROM OrderItems
GROUP BY order_num
HAVING SUM(item_price*quantity)>=1000
ORDER BY order_num;

--10.7.5
SELECT order_num, COUNT(*) AS items 
FROM OrderItems 
GROUP BY order_num --不能是items
HAVING COUNT(*) >= 3
ORDER BY items, order_num;



```
## Lesson 11 子查询
```sql
--Lesson11 子查询
SELECT cust_id
FROM Orders
WHERE order_num IN (SELECT order_num
					FROM OrderItems
					WHERE prod_id = 'RGAN01');
--对每个顾客执行COUNT(*)
SELECT cust_name, cust_state, 
		(SELECT COUNT(*) 
		 FROM Orders 
		 WHERE Orders.cust_id = Customers.cust_id) AS orders
FROM Customers
ORDER BY cust_name;

--11.5.1
SELECT cust_id
FROM Orders
WHERE order_num in (SELECT order_num
					FROM OrderItems
					WHERE item_price>=10 );

--11.5.2
SELECT cust_id,order_date
FROM Orders
WHERE order_num IN (SELECT order_num
					FROM OrderItems
					WHERE prod_id='BR01')
--11.5.3
SELECT cust_email
FROM Customers
WHERE cust_id IN (SELECT cust_id
				  FROM Orders
				  WHERE order_num IN (SELECT order_num
									  FROM OrderItems
									  WHERE prod_id='BR01'))
						
--11.5.4
--我们需要一个顾客 ID列表，其中包含他们已订购的总金额。
--编写SQL 语句，返回顾客 ID（Orders 表中的 cust_id）
--并使用子查询返回 total_ordered 以便返回每个顾客的订单总数。
--将结果按金额从大到小排序。提示：你之前已经使用 SUM()计算订单总数。
SELECT cust_id,
			(SELECT SUM(item_price*quantity)
			FROM OrderItems
			WHERE Orders.order_num=OrderItems.order_num)
			As total_ordered
FROM Orders
ORDER BY total_ordered DESC;

--11.5.5
SELECT prod_name,
		(SELECT SUM(quantity)
		 FROM OrderItems
		 WHERE Products.prod_id=OrderItems.prod_id)
		 AS quant_sold
FROM Products;
				  


```

## Lesson 12 联结表

```SQL
-- 联结表(join)

--12.2创建联结表
SELECT vend_name, prod_name, prod_price 
FROM Vendors, Products
WHERE Vendors.vend_id = Products.vend_id;
--如果没有WHERE，则第一个表中的每一行都与第二个表
--每一行匹配（笛卡尔积）

--内联结（同样写法）
SELECT vend_name, prod_name, prod_price 
FROM Vendors
INNER JOIN Products ON Vendors.vend_id = Products.vend_id;

--联结多个表
--联结越多表性能下降越厉害
SELECT prod_name, vend_name, prod_price, quantity 
FROM OrderItems, Products, Vendors
WHERE Products.vend_id=Vendors.vend_id
 AND OrderItems.prod_id=Products.prod_id
 AND order_num=20007;

 --12.4.1
SELECT cust_name,order_num
FROM Customers,Orders
WHERE Customers.cust_id=Orders.cust_id
ORDER BY cust_name,order_num;

SELECT cust_name,order_num
FROM Customers INNER JOIN Orders 
ON Customers.cust_id=Orders.cust_id
ORDER BY cust_name,order_num;
--12.4.2
--(1)子查询实现
SELECT cust_name,order_num,
				(SELECT SUM(item_price*quantity)
				 FROM OrderItems
				 WHERE OrderItems.order_num=Orders.order_num)AS OrderTotal--完全限定列名
FROM Customers
INNER JOIN Orders ON Customers.cust_id=Orders.cust_id
ORDER BY cust_name,order_num;


--(2)联结？
SELECT cust_name,Orders.order_num,SUM(item_price*quantity)AS OrderTotal--?
FROM Customers,Orders,OrderItems
WHERE Customers.cust_id=Orders.cust_id
	AND Orders.order_num =OrderItems.order_num
GROUP BY cust_name,Orders.order_num
ORDER BY cust_name,order_num;

--12.4.3
SELECT order_date
FROM Orders,OrderItems
WHERE OrderItems.order_num=Orders.order_num AND prod_id='BR01'

SELECT order_date
FROM Orders
INNER JOIN OrderItems ON OrderItems.order_num=Orders.order_num
WHERE prod_id='BR01'

--12.4.4
--多个联结
SELECT order_date,cust_email
FROM Orders
INNER JOIN OrderItems ON OrderItems.order_num=Orders.order_num 
INNER JOIN Customers ON Orders.cust_id=Customers.cust_id
WHERE prod_id='BR01'

--12.4.5
/*5. 再让事情变得更加有趣些，我们将混合使用联结、聚合函数和分组。 准备好了吗？
回到第 10课，当时的挑战是要求查找值等于或大于 1000 的所有订单号。
这些结果很有用，但更有用的是订单数量至少达到 这个数的顾客名称。
因此，编写 SQL 语句，使用联结从 Customers 表返回顾客名称（cust_name），
并从 OrderItems 表返回所有订单的 总价。 
提示：要联结这些表，还需要包括 Orders 表（因为 Customers 表 与 OrderItems 表不直接相关，
Customers 表与 Orders 表相关，而 Orders 表与 OrderItems 表相关）。
不要忘记 GROUP BY 和 HAVING， 并按顾客名称对结果进行排序。
你可以使用简单的等联结或 ANSI的INNER JOIN 语法。
或者，如果你很勇敢，请尝试使用两种方式编写。
*/
--是的，我很勇敢！
/*
--（1）简单的等联结
SELECT cust_name,SUM(quantity*order_item*item_price) AS TotalPrice,
				SUM(quantity*order_item) AS OrderQuan--订单数量
FROM Customers,Orders,OrderItems
WHERE Customers.cust_id=Orders.cust_id 
	AND OrderItems.order_num=Orders.order_num 
Group BY Customers.cust_name
HAVING SUM(quantity*order_item)>1000;

--（2）等联结
SELECT cust_name,sum(quantity*order_item*item_price) AS TotalPrice,
				SUM(quantity*order_item) AS OrderQuan--订单数量
FROM Orders
INNER JOIN Customers ON Customers.cust_id=Orders.cust_id 
INNER JOIN OrderItems ON OrderItems.order_num=Orders.order_num 
Group BY Customers.cust_name
HAVING SUM(quantity*order_item)>1000;

*/
--题解写的是返回订单总价>=1000
SELECT cust_name,SUM(quantity*item_price) AS TotalPrice,
				SUM(quantity*item_price) AS OrderQuan--订单数量
FROM Customers,Orders,OrderItems
WHERE Customers.cust_id=Orders.cust_id 
	AND OrderItems.order_num=Orders.order_num 
Group BY Customers.cust_name
HAVING SUM(quantity*item_price)>=1000;


```
## Lesson 13 高级联结

### 习题

1. 使用 INNER JOIN 编写 SQL语句，以检索每个顾客的名称（Customers
   表中的 cust_name）和所有的订单号（Orders 表中的 order_num）

```sql
select cust_name,order_num
from Customers
inner join Orders on orders.cust_id=Customers.cust_id
```

2. 修改刚刚创建的 SQL 语句，仅列出所有顾客，即使他们没有下过订单。 

   ```sql
   select cust_name
   from Customers
   left outer join Orders on orders.cust_id=Customers.cust_id
   ```

   

3. 使用 OUTER JOIN 联结 Products 表和 OrderItems 表，返回产品名 称（prod_name）和与之相关的订单号（order_num）的列表，并按 商品名称排序。 

   ```sql
   select prod_name,order_num
   from Products
   left outer join orderItems on Products.prod_id=OrderItems.prod_id
   order by prod_name
   ```

   

4. 修改上一题中创建的 SQL 语句，使其返回每一项产品的总订单数 （不是订单号）。 

   

    ```sql
    select count(order_num) as orders
    from Products
    left outer join orderItems on Products.prod_id=OrderItems.prod_id
    group by Products.prod_id
    ```



5. 编写 SQL语句，列出供应商（Vendors 表中的 vend_id）及其可供产品 的数量，包括没有产品的供应商。你需要使用 OUTER JOIN 和 COUNT() 聚合函数来计算 Products 表中每种产品的数量。注意：vend_id 列 会显示在多个表中，因此在每次引用它时都需要完全限定它

   ```sql
   select Vendors.vend_id,COUNT(prod_id)
   from Vendors
   left outer join Products on Products.vend_id=Vendors.vend_id
   group by Vendors.vend_id
   ```
   
   



## Lesson 15 插入数据

### 插入一行
```sql
INSERT INTO  Customers(cust_id,
					   cust_name,
                       cust_address,
                       cust_city,
                       cust_state,
                       cust_zip,
                       cust_country,
                       cust_contact,
					   cust_email)
--每一列必须提供一个值
VALUES(1000000006,
		'Toy Land',
        '123 Any Street',
        'New York',
        'NY', 
        '11111',
        'USA',
		NULL,
		NULL);--不能写""
```
### 插入部分
```sql
INSERT INTO  Customers(cust_id,
					   cust_name,
                       cust_address,
                       cust_city,
                       cust_state,
                       cust_zip,
                       cust_country)
VALUES(1000000006,
		'Toy Land',
        '123 Any Street',
        'New York',
        'NY', 
        '11111',
        'USA',);
```
如果某列定义为允许NULL，或表定义中给出了默认值则可以省略
### 插入检索出的数据
从一个名为CustNew的表中读出数据并插入到Customers
```sql
INSERT INTO Customers(cust_id, 
					  cust_contact,
                      cust_email,
                      cust_name,
                      cust_address,
                      cust_city,
                      cust_state,
                      cust_zip,
                      cust_country)
SELECT cust_id, 
	   cust_contact,
	   cust_email,
	   cust_name,
       cust_address,
       cust_city,
       cust_state,
       cust_zip,
       cust_country
FROM CustNew;
```
### 复制表
`SELECT * INTO CustCopy FROM Customers;`
-  任何 SELECT 选项和子句都可以使用，包括 WHERE 和 GROUP BY； 
-  可利用联结从多个表插入数据；
-  不管从多少个表中检索数据，数据都只能插入到一个表中。

### 15.4 习题答案
```sql
--1.
INSERT INTO Customers(cust_id,
					  cust_name,
					  cust_address,
					  cust_city,
					  cust_state,
					  cust_country)
VALUES(1000000007,'ME','Sichuan','Chengdu',
				'CN','CN')
--2.
SELECT * INTO Orders_backup FROM Orders;
SELECT * INTO OrdersItems_backup FROM OrderItems;

```



## Lesson 18 视图
```sql

--使用视图
--视图是虚拟的表，包含一个查询
--与表一样，视图必须唯一命名（不能给视图取与别的视图或表相同的 名字）。
--对于可以创建的视图数目没有限制。
--创建视图，必须具有足够的访问权限。这些权限通常由数据库管理人员授予。
--视图可以嵌套，即可以利用从其他视图中检索数据的查询来构造视图。
--所允许的嵌套层数在不同的DBMS中有所不同（嵌套视图可能会严重降低查询的性能，因此在产品环境中使用之前，应该对其进行全面测试）。

--限制和规则
--许多DBMS禁止在视图查询中使用 ORDER BY 子句。

--18.2.1使用视图简化复杂的联结
/*
DROP VIEW IF EXISTS ProductCustomers
CREATE VIEW ProductCustomers AS --创建视图
SELECT cust_name, cust_contact, prod_id
FROM Customers, Orders, OrderItems
WHERE Customers.cust_id = Orders.cust_id
AND OrderItems.order_num = Orders.order_num;


--删除视图   DROP VIEW viewname;

--检索订购了产品 RGAN01 的顾客，可如下进行：
SELECT cust_name, cust_contact
FROM ProductCustomers
WHERE prod_id = 'RGAN01';

CREATE VIEW VendorLocations AS
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')'
AS vend_title
FROM Vendors;


SELECT * FROM VendorLocations;
*/

--视图提供了一种封装 SELECT 语句的层次，可用来简化数据处理，重新 格式化或保护基础数据。

--18.4.1
/*
CREATE VIEW CustomersWithOrders AS
SELECT Customers.cust_id,cust_name,cust_address,
cust_city,cust_contact,cust_email,
cust_state,cust_zip,cust_country
FROM Customers
INNER JOIN Orders ON Orders.cust_id=Customers.cust_id
*/
SELECT *
FROM CustomersWithOrders
```

## Lesson 19 存储过程

为以后使用而保存的一条或多条SQL语句，可将其视为批文件。

### 19.3执行存储过程

EXECUTE:

可能有以下执行选择：

- 参数可选

- 不按次序给出参数

- 输出参数

- 用SELECT语句检索数据

- 返回代码，允许存储过程返回一个值到正在执行的应用程序

  

###19.4 创建存储过程
```sql
CREATE PROCEDURE MailingListCount 
AS 
DECLARE @cnt INTEGER 
SELECT @cnt = COUNT(*) 
FROM Customers WHERE NOT cust_email IS NULL;
RETURN @cnt;
```
调用:
```SQL
DECLARE @ReturnValue INT 
EXECUTE @ReturnValue=MailingListCount;
SELECT @ReturnValue;
```

