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

用意：若記錄內容完全相同，只留一筆

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

### 列出前幾筆的資料

語法：ORDER BY 排序欄位 LIMIT 前Ｎ筆資料;

ex：列出前五筆資料
```
ORDER BY quantity LIMIT 5;
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

### COUNT與Null 值
* 絕大多數的彙總函數均排除Null，不列入計算
* COUNT(*) 例外，有Null 值的資料仍然計入一筆




