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
* FROM 指定資料表名稱
* ORDER BY 指定排序欄位
* SELECT 表示要顯示的欄位
* WHERE 指定篩選欄位

USE 資料庫名稱
SELECT 要顯示的欄位
FROM 要使用的資料表名稱
WHERE 篩選方式
ORDER BY 要顯示的排序方式

## ex:指定欄位清單
```
USE northwind;
SELECT employeeid, lastname, firstname, title
FROM employees;

```
## ex:資料排序
```
USE northwind;
SELECT productid, productname, categoryid, unitprice
FROM products
ORDER BY categoryid, unitprice DESC;
--ASC代表由小排到大
--DESC代表由大排到小
```

## h2篩選資料WHERE句型

### 比較型









