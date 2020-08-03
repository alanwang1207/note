# MYSQL

關聯式資料庫邏輯結構

## Foreign Key，FK
必要時，以某個欄位為外鍵（Foreign Key，FK）關聯到另一資料表的主鍵以獲得進一步的相關資料
## Primary Key，PK
每個資料表都各有其主鍵

橫列稱為記錄（Record）

直欄稱為欄位（Field）
## SELECT 敘述基本語法
* <select_list>: 以逗號條列各個欄位
* USE 資料庫名稱
* SELECT 表示要顯示的欄位
* FROM 指定資料表名稱
* WHERE 指定篩選欄位
* ORDER BY 指定排序欄位



## ex:指定欄位清單
```
USE northwind;
SELECT employeeid, lastname,
firstname, title
FROM employees;

```
## ex:資料排序
```
USE northwind;
SELECT productid, productname,
categoryid, unitprice
FROM products
ORDER BY categoryid, unitprice DESC;
--ASC代表由小排到大
--DESC代表由大排到小
```

## 篩選資料WHERE句型

### 比較型

WHERE 欄位名稱 = 值 or '名'

### 樣式比對型

WHERE 比較名稱 LIKE 'a%'

字母不分大小寫,如需區分可在LIKE後加上BINARY

WHERE 欄位名稱 LIKE BINARY 'A%'

| 'a%'     | '%a'     |  '%a%'|
| -------- | -------- | -------- |
| 以a開頭   |以a結尾    | 可以a在任何位置|    

_可限制字的長度_可代表一個中文字元   
```
ex:'_a' 兩位且以a結尾
```     

### 可運用邏輯運算元

```
USE northwind;
SELECT productid, productname, supplierid, unitprice
FROM  productsWHERE
(productname LIKE 'T%' OR productid = 46) 
AND(unitprice > 16.00) ;
```

### 區間型

WHERE 欄位名稱 BETWEEN 10 AND 20;


```
WHERE unitprice BETWEEN 10 AND 20;

--介於10至20之間
```

### 列舉型

WHERE 欄位名稱 IN (欄位值);

```
WHERE country IN ('Japan', 'Italy');

--國家包含Japan 或是Italy
```

### NULL= Unknown「未知」

WHERE 欄位名稱 IS NULL;
      
```
WHERE fax IS NULL;

--顯示fax欄位值為NULL
```      

### DISTINCT

用意：若橫向記錄內容完全相同，只留一筆

語法：SELECT DISTINCT 欄位值

ex：顯示國家種類
```
SELECT DISTINCT country
```

### 變更欄位名稱

語法：SELECT 欄位名稱 AS 要改變的欄位名

ex：將firstname改變成First
```
SELECT firstname AS First
```
ex:將(a*b)改成ｃ
```
SELECT (a*b) AS c
WHERE c >= 100
如果找不到c則使用舊名稱（a*b）
```
如果欄位ID是中文或有空格
前後加入、、去做修飾

ex:將ProductID 用產品編號表示
```
SELECT ProductID AS `產品編號`
```



### 靜態文字修飾資料

Concat()函數是用來連接兩個字符串，形成一個字符串，連接方式使用逗號。

語法：Concat(str1,str2,…) 

MySQL 的CONVERT()函數可用來獲取一個類型的值，並產生另一個類型的值。

語法：CONVERT(value, type);

```
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
```
ORDER BY quantity LIMIT 5;
```
ex:一頁十筆資料,列出第三頁的十筆資料
```
ORDER BY 欄位名稱  LIMIT 20,10

```

limit N :返回N條記錄

offset M :跳過M條記錄,默認M=0,單獨使用似乎不起作用

limit N,M :相當於limit M offset N ,從第N條記錄開始,返回M條記錄


### 子查詢

基本語法:
```
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

這裡使用FK(supplierID)來幫助查詢(用Ｂ表幫Ａ表做事)

使用in,select只能一個欄位

```
SELECT categoryID, p.categoryID,
ProductID, ProductName,UnitPrice,
UnitPrice-(SELECT AVG(UnitPrice) 
FROM products 
WHERE categoryID = p.categoryID) 
AS DiffPrice 
FROM products AS p
```
上述先把針對products的子查詢結果視同一個名為p的資料表，
接著計算出DiffPrice(子查詢)，最後列出須要顯示的項目
:::info
p.categoryID 是為了證明與categoryID相同
:::

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








### JOIN 結合多個資料表
語法：
* 以JOIN 註明另一個資料表
* 再以ON 指定結合條件（通常利用主鍵與外鍵欄位指定ON 條件）
* 欄位名稱重複時，須加註資料表名稱（或別名）
* OUTER為以外的意思 ex. LEFT OUTER 表示左邊以外
```
mysql> 
SELECT A.id,A.name,B.name 
FROM A,B 
WHERE A.id=B.id;
---- ----------- ------------- 
| id | name       | nam        |
---- ----------- ------------- 
|  1 | Pirate     | Rutabaga   |
|  2 | Monkey     | Pirate     |
|  3 | Ninja      | Darth Vader|
|  4 | Spaghetti  | Ninja      |
---- ----------- ------------- 
4 rows in set (0.00 sec)
```
JOIN 按照功能大致分為如下三類：



### Inner join
內連線，也叫等值連線，inner join產生同時符合A和B的一組資料。
```
mysql> 
SELECT * 
FROM A inner join B on A.name = B.name;
---- -------- ---- -------- 
| id | name   | id | name   |
---- -------- ---- -------- 
|  1 | Pirate |  2 | Pirate |
|  3 | Ninja  |  4 | Ninja  |
---- -------- ---- -------- 
```

### Left join
left join,（或left outer join:在Mysql中兩者等價，推薦使用left join.）左連線從左表(A)產生一套完整的記錄,與匹配的記錄(右表(B)) .如果沒有匹配,右側將包含null。
```
mysql> 
SELECT * 
FROM A left join B on A.name = B.name;
#或者：
SELECT * 
FROM A left outer join B on A.name = B.name;
---- ----------- ------ -------- 
| id | name      | id   | name   |
---- ----------- ------ -------- 
|  1 | Pirate    |    2 | Pirate |
|  2 | Monkey    | NULL | NULL   |
|  3 | Ninja     |    4 | Ninja  |
|  4 | Spaghetti | NULL | NULL   |
---- ----------- ------ -------- 
4 rows in set (0.00 sec)
```

如果想只從左表(A)中產生一套記錄，但不包含右表(B)的記錄，可以通過設定where語句來執行，如下：
```
mysql> 
SELECT * 
FROM A left join B on A.name=B.name 
WHERE A.id is NULL or B.id is NULL;
---- ----------- ------ ------ 
| id | name      | id   | name |
---- ----------- ------ ------ 
|  2 | Monkey    | NULL | NULL |
|  4 | Spaghetti | NULL | NULL |
---- ----------- ------ ------ 
2 rows in set (0.00 sec)
```

同理，還可以模擬inner join. 如下：

```
mysql> 
SELECT * 
FROM A left join B on A.name=B.name 
WHERE A.id is not NULL and B.id is not NULL;
---- -------- ------ -------- 
| id | name   | id   | name   |
---- -------- ------ -------- 
|  1 | Pirate |    2 | Pirate |
|  3 | Ninja  |    4 | Ninja  |
---- -------- ------ -------- 
2 rows in set (0.00 sec)
```

### 求差集：

```
SELECT * 
FROM A LEFT JOIN B ON A.name = B.name
WHERE B.id IS NULL union
SELECT * 
FROM A right JOIN B ON A.name = B.name
WHERE A.id IS NULL;

------ ----------- ------ ------------- 
| id   | name      | id   | name        |
------ ----------- ------ ------------- 
|    2 | Monkey    | NULL | NULL        |
|    4 | Spaghetti | NULL | NULL        |
| NULL | NULL      |    1 | Rutabaga    |
| NULL | NULL      |    3 | Darth Vader |
------ ----------- ------ ------------- 
```

### Right join

```
mysql> 
SELECT * 
FROM A right join B on A.name = B.name;
------ -------- ---- ------------- 
| id   | name   | id | name        |
------ -------- ---- ------------- 
| NULL | NULL   |  1 | Rutabaga    |
|    1 | Pirate |  2 | Pirate      |
| NULL | NULL   |  3 | Darth Vader |
|    3 | Ninja  |  4 | Ninja       |
------ -------- ---- ------------- 
4 rows in set (0.00 sec)
```
同left join。

### Cross join
交叉連線，得到的結果是兩個表的乘積，即笛卡爾積
```
mysql> 
SELECT * 
FROM A cross join B;
---- ----------- ---- ------------- 
| id | name      | id | name        |
---- ----------- ---- ------------- 
|  1 | Pirate    |  1 | Rutabaga    |
|  2 | Monkey    |  1 | Rutabaga    |
|  3 | Ninja     |  1 | Rutabaga    |
|  4 | Spaghetti |  1 | Rutabaga    |
|  1 | Pirate    |  2 | Pirate      |
|  2 | Monkey    |  2 | Pirate      |
|  3 | Ninja     |  2 | Pirate      |
|  4 | Spaghetti |  2 | Pirate      |
|  1 | Pirate    |  3 | Darth Vader |
|  2 | Monkey    |  3 | Darth Vader |
|  3 | Ninja     |  3 | Darth Vader |
|  4 | Spaghetti |  3 | Darth Vader |
|  1 | Pirate    |  4 | Ninja       |
|  2 | Monkey    |  4 | Ninja       |
|  3 | Ninja     |  4 | Ninja       |
|  4 | Spaghetti |  4 | Ninja       |
---- ----------- ---- ------------- 
16 rows in set (0.00 sec)
```
實際上，在 MySQL 中（僅限於 MySQL） CROSS JOIN 與 INNER JOIN 的效果是一樣的，在不指定 ON 條件得到的結果都是笛卡爾積，反之取得兩個表完全匹配的結果。
INNER JOIN 與 CROSS JOIN 可以省略 INNER 或 CROSS 關鍵字，因此下面的 SQL 效果是一樣的：
```
... FROM table1 INNER JOIN table2
... FROM table1 CROSS JOIN table2
... FROM table1 JOIN table2
```

### Full join
全連線產生的所有記錄（雙方匹配記錄）在表A和表B。如果沒有匹配,則對面將包含null。
```
mysql> 
SELECT * 
FROM A left join B on B.name = A.name 
-> union 
-> SELECT * 
FROM A right join B on B.name = A.name;
------ ----------- ------ ------------- 
| id   | name      | id   | name        |
------ ----------- ------ ------------- 
|    1 | Pirate    |    2 | Pirate      |
|    2 | Monkey    | NULL | NULL        |
|    3 | Ninja     |    4 | Ninja       |
|    4 | Spaghetti | NULL | NULL        |
| NULL | NULL      |    1 | Rutabaga    |
| NULL | NULL      |    3 | Darth Vader |
------ ----------- ------ ------------- 
6 rows in set (0.00 sec)
```

:::info
注意：mysql不支援Full join,不過可以通過UNION 關鍵字來合併 LEFT JOIN 與 RIGHT JOIN來模擬FULL join.
:::

50題 練習題１
https://kknews.cc/zh-tw/code/5nny5b2.html

50題 練習題２
https://blog.csdn.net/flycat296/article/details/63681089