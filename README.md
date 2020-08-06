# MYSQL

關聯式資料庫邏輯結構

### Foreign Key，FK
必要時，以某個欄位為外鍵（Foreign Key，FK）關聯到另一資料表的主鍵以獲得進一步的相關資料
### Primary Key，PK
每個資料表都各有其主鍵

橫列稱為記錄（Record）

直欄稱為欄位（Field）
### SELECT 敘述基本語法
* USE 資料庫名稱
* SELECT 表示要顯示的欄位
* <select_list>: 以逗號條列各個欄位
* FROM 指定資料表名稱
* WHERE 指定篩選欄位
* ORDER BY 指定排序欄位



#### ex:指定欄位清單
```sql
USE northwind;
SELECT employeeid, lastname,
firstname, title
FROM employees;

```
#### ex:資料排序
```sql
USE northwind;
SELECT productid, productname,
categoryid, unitprice
FROM products
ORDER BY categoryid, unitprice DESC;
--ASC代表由小排到大
--DESC代表由大排到小
```

### 篩選資料WHERE句型

#### 比較型

WHERE 欄位名稱 = 值 or '名'

#### 樣式比對型

WHERE 比較名稱 LIKE 'a%'

字母不分大小寫,如需區分可在LIKE後加上BINARY

WHERE 欄位名稱 LIKE BINARY 'A%'

| 'a%'     | '%a'     |  '%a%'|
| -------- | -------- | -------- |
| 以a開頭   |以a結尾    | 可以a在任何位置|    

_可限制字的長度_可代表一個中文字元   
```sql
ex: '_a' 兩位且以a結尾
```     

#### 可運用邏輯運算元

```sql
USE northwind;
SELECT productid, productname, supplierid, unitprice
FROM  products
WHERE(productname LIKE 'T%' OR productid = 46) 
AND(unitprice > 16.00) ;
```

#### 區間型

```sql

WHERE 欄位名稱 BETWEEN 10 AND 20;

WHERE unitprice BETWEEN 10 AND 20;

--介於10至20之間
```

日期查詢也可用區間型

```sql
SELECT * 
FROM 表單名稱
WHERE date 
BETWEEN '2018-05-10' AND '2018-08-20';
```

#### 列舉型

WHERE 欄位名稱 IN (欄位值);

```sql
WHERE country IN ('Japan', 'Italy');

--國家包含Japan 或是Italy
```

### NULL= Unknown「未知」

```sql
WHERE 欄位名稱 IS NULL;
      
WHERE fax IS NULL;

--顯示fax欄位值為NULL
```      

### DISTINCT

用意：若橫向記錄內容完全相同，只留一筆

語法：SELECT DISTINCT 欄位值

ex：顯示國家種類
```sql
SELECT DISTINCT country
```

### 變更欄位名稱

語法：SELECT 欄位名稱 AS 要改變的欄位名

ex：將firstname改變成First
```sql
SELECT firstname AS First
```
ex:將(a*b)改成ｃ
```sql
SELECT (a*b) AS c
WHERE c >= 100
如果找不到c則使用舊名稱（a*b）
```
如果欄位ID是中文或有空格
前後加入、、去做修飾

ex:將ProductID 用產品編號表示
```sql
SELECT ProductID AS `產品編號`
```



### 靜態文字修飾資料

Concat()函數是用來連接兩個字符串，形成一個字符串，連接方式使用逗號。

語法：Concat(str1,str2,…) 

MySQL 的CONVERT()函數可用來獲取一個類型的值，並產生另一個類型的值。

語法：CONVERT(value, type);

```sql
USE northwind;
SELECT Concat('Identification number:',
Convert(employeeid, varchar(2)) 
as ID
FROM employees;


| ID                        | 
---------------------------- 
| Identification Number:  1 | 
| Identification Number:  2 | 
| Identification Number:  3 |  

```


### 列出前幾筆的資料

語法：ORDER BY 排序欄位 LIMIT 前Ｎ筆資料;

ex：列出前五筆資料
```sql
ORDER BY quantity LIMIT 5;
```
ex:一頁十筆資料,列出第三頁的十筆資料
```sql
ORDER BY 欄位名稱  LIMIT 20,10
```


limit N :返回N條記錄

offset M :跳過M條記錄,默認M=0,單獨使用似乎不起作用

limit N,M :相當於limit M offset N ,從第N條記錄開始,返回M條記錄


### 子查詢

基本語法:
```sql
SELECT 欄位名稱1,欄位名稱2,...,欄位名稱n 
FROM 資料表名稱1
WHERE 欄位名稱 = 
(SELECT 欄位名稱 FROM 資料表名稱2 WHERE 條件)
```
1. 子查詢要在括號()中。
2. 通常子查詢SELECT只會取得單一欄位的值，以便主查詢的欄位進行比較運算。
3. 子查詢通常使用時機為加強篩選條件，使用外部鍵(FK)與其他表作間接查詢
5. 如需排序，子查詢不能使用ORDER BY，只能使用GROUP BY 子句。
6. 如果子查詢可以取得多筆資料，在主查詢需使用IN邏輯運算子。


ex.美商供應的產品清單

* 起

SELECT * FROM products
* 承

SELECT ProductID, ProductName, UnitPrice
FROM products

WHERE ??? 不知道哪國的商人
* 轉

SELECT * 
FROM suppliers 
WHERE Country = 'USA'

取得SupplierID
* 合

SELECT ProductID, ProductName, UnitPrice
FROM products
WHERE SupplierID in 
(SELECT SupplierID 
FROM Suppliers 
WHERE Country = 'USA')

這裡使用FK(supplierID)來幫助查詢(用B表幫A表做事)

使用in,select只能一個欄位

<br>

* 查詢的本身可作為（相同環境下）子查詢的條件

```sql
SELECT categoryID, p.categoryID,
ProductID, ProductName,UnitPrice,
UnitPrice-(SELECT AVG(UnitPrice) 
FROM products 
WHERE categoryID = p.categoryID) 
AS DiffPrice 
FROM products AS p
```
上述先把針對products的查詢結果視同一個名為p的資料表，
接著計算出DiffPrice(子查詢)，最後列出要顯示的項目
:::info
p.categoryID 是為了證明與categoryID相同
:::
<br>

* 將子查詢的結果視同為一個資料表
```sql
SELECT  * FROM (SELECT OrderID, OrderDate 
FROM Orders ORDER BY OrderDate DESC limit 10) AS T
ORDER BY OrderDate ASC
```

### 彙總函數

| 函數名稱 | 功能描述|
| -------- | -------- |
| AVG | 計算平均值 |
| COUNT | 有資料的共有幾筆 |
| COUNT (*) | 一共多少筆（有Null值的記錄也算進去） |
| MAX | 傳回最大值 |
| MIN | 傳回最小值 |
| SUM | 計算總和 |
| STDEV | 計算標準差 |
| VAR | 計算變異數|

ex:avg(欄位名稱)

計算限制的欄位ex:avg(UnitPrice)from products where categroyID = 1

### COUNT與Null 值
* 絕大多數的彙總函數均排除Null，不列入計算
* COUNT(*) 例外，有Null 值的資料仍然計入一筆
```sql
ex:SELECT COUNT (欄位名稱) FROM 資料表名稱;
```


### 使用GROUP BY子句

* 搭配AVG()、COUNT()、MAX()、MIN()、SUM() 等聚合函數使用，用來將查詢結果中特定欄位值相同的資料分為 干個群組，而每一個群組都會傳回一個資料列。
```sql
SELECT column_name(s), aggregate_function(column_name)
FROM table_name
GROUP BY column_name1, column_name2...;
```
* 使用HAVING篩選結果集的資料列(放在GROUP BY後)
```sql
SELECT categoryid, AVG(UnitPrice) 
FROM products 
GROUP BY CategoryID HAVING AVG(UnitPrice) >= 30
```
1. 沒有GROUP BY的時候，通常使用WHERE而不使用HAVING
2. 含有HAVING子句的SQL並不一定要包含GROUP BY
3. WHERE置於GROUP BY前面，只有符合WHERE子句條件的資料列才會被分組

<br>

* GROUP BY 後可以跟WITH ROLLUP，表示在進行分組統計的基礎上再次進行彙總統計（在每個分組下都會有統彙總）：
```sql
SELECT orderid,productID,SUM(quantity) 
FROM `order details`
GROUP BY orderid,ProductID 
WITH ROLLUP
```
[MySQL GROUP BY ROLLUP 的應用](https://ithelp.ithome.com.tw/articles/10136825)
:::info
沒有在GROUP BY後,且無經過彙整函數的欄位名稱無法寫在SELECT後方
:::

```sql
ex:SELECT categoryID, ~~productID~~, AVG(UnitPrice)

FROM products

GROUP BY categoryID
```




[SQL中ORDER BY和GROUP BY的區別](https://www.cnblogs.com/klb561/p/11657962.html)

### JOIN 結合多個資料表
語法：
```sql
SELECT column1, column2
FROM tableA A   // = as A
JOIN tableB B 
ON A.columnName = B.columnName 或 USING (columnName)
```
* 以JOIN 註明另一個資料表
* 再以ON 指定結合條件（通常利用主鍵與外鍵欄位指定ON 條件）
* 欄位名稱重複時，須加註資料表名稱（或別名）
* OUTER為以外的意思 ex. LEFT OUTER 表示左邊以外
* 若用OUTER JOIN，以小的去比對大的較有效率


[JOIN範例](https://justcode.ikeepstudying.com/2016/08/mysql-%E5%9B%BE%E8%A7%A3-inner-join%E3%80%81left-join%E3%80%81right-join%E3%80%81full-outer-join%E3%80%81union%E3%80%81union-all%E7%9A%84%E5%8C%BA%E5%88%AB/)


ex:[老師範例](https://docs.google.com/spreadsheets/d/1YUNZHCl3sY8iTLd80ecNs7DgQ-8nA6xWCI7CLRzmE4U/edit?usp=sharing)



### UNION

UNION用於合併兩個或多個SELECT語句的結果，要求必須有相同數量的列、相似的數據類型，列的順序必須相同

```sql
SELECT 列 FROM 表1

UNION

SELECT 列 FROM 表2
```

:::info
注意：UNION默認選取不同值，允許重複則使用UNION ALL（合集）
INTERSECT（交集）
EXCEPT（差集）
:::

### 資料操作（DML）

#### INSERT
* 插入

```sql
INSERT INTO 表 VALUES （值，值）

INSERT INTO 表 （列，列） VALUES （值，值）
```
#### UPDATE
* 修改

```sql
UPDATE 表 SET 列 = 新值 WHERE 條件

ex：UPDATE Person SET Address = '張三' , City = '台北' WHERE T_id = 3
```

#### DELETE
* 删除行

```sql
DELETE FROM 表 WHERE 列 = 值

DELETE FROM 表 或 DELETE * FROM 表可以删除所有行

DELETE FROM 沒寫 WHERE條件，有機會刪除整份資料表內容（視有無其他相關的關聯）
```

#### TRUNCATE TABLE
* 清空資料表內容並**保留結構**
* 無法ROLLBACK

```sql
TRUNCATE TABLE 
```



<br>

### 資料定義

#### 建立資料庫
```sql
CREATE DATABASE [IF NOT EXISTS]名稱
[DEFAULT]CHARACTER SET utf8(字元編碼);
```
[IF NOT EXISTS] 表示不存在才會創建。建議在sql腳本中使用create命令創建數據庫時加入此項，以免對應名稱的數據庫已經存在導致sql腳本終止，為可選項

[DEFAULT]如果使用了default，這個數據庫中創建的所有資料表默認都會繼承這個數據庫的字符集，為可選項。
#### 刪除資料庫
```sql
drop database 資料庫名稱
```
#### 建立資料表
```sql
CREATE [TEMPORARY] TABLE 名稱 (欄位名稱,欄位型態,欄位選項)
```
[TEMPORARY]（暫時的資料表）會在連線之後就消失，為可選項。

#### 刪除資料表
```sql
drop table 資料表名稱
```

[MYSQL建立資料表](https://blog.xuite.net/hsiung03/blog/64202615-MYSQL+%E5%BB%BA%E7%AB%8B%E8%B3%87%E6%96%99%E8%A1%A8)

<br>

#### 修改資料表結構
* 加入新的欄位
```sql
alter table 資料表名稱
add 新欄位
```
* 變更欄位定義
```sql
alter table 資料表名稱
modify 欄位名稱 欄位屬性 (default 預設值)
```
:::info
如果修改屬性，原本有預設也要一起設定，否則會消失
:::
* 刪除欄位
```sql
alter table 資料表名稱
drop column 欄位名稱
```
* 處理同資料欄位
```sql=1
ex:
insert into t1 (id, data) values (1,100), (2,100);
insert into t1 (id, data) values (1,100)

alter table t1 add tempID int auto_increment primary key;
update t1 set id =3 where tempID = 3;
```
建立主鍵編號後即可修改資料

<br>



### 資料庫正規化

[資料庫正規化](https://hackmd.io/@alanwang1207/dbnorm)
 < 挪過去了

<br>

### 索引
以空間換取時間加快查詢速度，不建議用於有頻繁更新或插入操作的資料表。
#### 建立索引
```sql
CREATE INDEX 索引名 ON 表格名 (欄位名,...);
```
#### 查詢索引
```sql
SELECT * FROM information_schema.statistics 
where table_schema = '資料庫名';
```
```sql
SHOW INDEX FROM 表格名;
```
#### 刪除索引
```sql
DROP INDEX 索引名 ON 表格名; 
```
[索引的設計](https://ithelp.ithome.com.tw/articles/10221971)

[索引簡介](https://medium.com/@michael80402/mysql%E7%B4%A2%E5%BC%95-e002f707a5f4)


#### 索引練習測試

新增
```sql
CREATE INDEX idx_name ON customers(customerid) 
```
刪除
```sql
DROP INDEX idx_name ON customers
```
查詢
```sql
SELECT * FROM information_schema.statistics 
where table_schema = 'northwind'
```
測試用的 join-index
```sql=1
select c.customerid,companyname,o.orderid,p.productID,ProductName


from customers c join orders o on o.customerid =c.customerid
                 join `order details` od on od.orderid =o.orderid
                 join products p on p.ProductID = od.productID
order by c.customerid,p.productID
```
```sql
create index idx_Products_ProductID on Products(ProductID);
create index idx_OrderDetails_ProductID on `Order Details`(ProductID);
```
    
<br>

### 條件約束
#### check
Mysql version > 8.0 (教室5.7 爛！！！)

```sql
alter TABLE 資料表名稱 
add CONSTRAINT 約束名 check (條件)

資料在約束後 add 跟 update 都會被限制
```

#### indexs
欄位值不得重複：
```sql
create unique index idx_company on customer(companyName)
```
#### foreign key
```
| customer:                | orders:
+------------+-------------+---------+------------+
| customerId | companyName | orderId | customerId |
+------------+-------------+---------+------------+
|          1 | A           |       1 |          2 |
|          2 | B           |       2 |          1 |
+------------+-------------+---------+------------+
```
```sql
alter table orders 
add constraint fk_customer_orders
foreign key (customerId) references customer(customerId)

on update cascade
on delete cascade
```
:::info
  on update/delete cascade
 
 可加可不加,加入後如果修改外鍵就會跟著修改,如果沒加,則完全不更動
:::

### SHOW

#### 終端機登入路徑
```shell
/Applications/MAMP/Library/bin/mysql -uroot -p
```
密碼：root

#### 顯示資料夾路徑
```sql
SHOW VARIABLES LIKE 'data%';
```
#### 顯示編碼種類
```sql
SHOW VARIABLES LIKE 'char%';
```
#### 顯示資料庫、資料表
```sql
SHOW DATABASES ;
USE database_name ;
SHOW TABLES ; 
DESCRIBE table_name ;
```
#### 資料夾路徑
```shell
/Applications/MAMP/db/mysql57/
```
### UTF8 與 BIG5轉換
![](https://i.imgur.com/NnA65Vd.png)

#### 設定位元組
```sql
SET NAMES big5 ;
```
<br>

### 使用者權限

#### 授與一個新的使用者
```sql
grant usage on *.* to 使用者名稱@'IP位址' identified by '密碼';
```

#### 新增一個新的使用者
```sql
create user 使用者名稱 identified by '密碼';
```
#### 授與/移除使用者使用資料庫權限
```sql
grant SELECT on 資料庫名稱.資料表名稱 to 使用者名稱

revoke SELECT on 資料庫名稱.資料表名稱 from 使用者名稱

可以選擇授與 SELECT or UPDATE
```


-- 請試著描述 user, db, tables_priv 等資料表的作用？

user ：查看使用者資料，紀錄使用者權限

db ：各個使用者對每個資料庫的權限

table_priv ：紀錄使用者對特定表內操作的權限

<br>

*user表中的密碼欄位-> authentication_string

### 參考資料
[50題 練習題１](https://kknews.cc/zh-tw/code/5nny5b2.html)

[50題 練習題２](https://blog.csdn.net/flycat296/article/details/63681089)

[SQL筆記](https://github.com/littlefis/SQLNote/blob/master/SQL.md)

[SQL筆記](https://github.com/bfsz/SQLNote)

[PHP](https://hackmd.io/@alanwang1207/php)

[前端大神github](https://github.com/Dent24/Toby_YUAN)

[repo](https://github.com/alanwang1207/note)

[MD語法](https://hackmd.io/@emisjerry/ByMQ0rWIB?type=slide#/)
    
