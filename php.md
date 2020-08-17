# PHP
變數前加＄符號


### 輸出
```php
<?php echo?> 或 <?= ?>
也可用
<?php print?>
```
echo 與 print 差異
1. echo 比 print快一點
2. echo 不回傳資料，print永遠回傳1
3. echo,print 都不是一個函數，而是一個結構construct，所以後面的參數不加小括號 `()`
4. echo 可傳入多個參數，print不行

### 型態
#### String
單引號與雙引號：純文字字串建議使用`''`去做文字的包覆(與其他傳統語言不同)
```php
雙引號中的變數會自動代入變數內容
$foo = 2;
echo "foo is $foo\n"; // 結果: foo is 2(換行)
echo 'foo is $foo\n'; // 結果: foo is $foo\n
```

:::info
雙引號串中的內容可以被解釋而且替換，而單引號串中的內容總被認為是普通字符<br>
:::
字串相連使用 `.` 句號
#### Boolean(布林)

$YesNo = TRUE

0與"0" => FALSE
1與"1" => TRUE


#### TimeStamp（時間戳記）
```php
<?= date("Y-y-M-m-D-d"); ?>
//2020-20-Aug-08-Mon-17
```
### 定義常數
#### define
`define()`函式宣告常數,常數能是數值的值，包括布林、整數、浮點數和字串，雖然也可以設為資源，但有可能會出現問題。

```php
define("常數名稱",值);
```
* 使用方式-直接用常數名稱不用加雙引號
#### const

#### define 與 const 差異
* define 可用在條件判斷中，不成立的條件中，定義的不生效，成功定義後全域性可用，可表達式賦值

* const 不可用在條件判斷中，不過可定義在class中，不可表達式賦值，必須是標量
### 引用檔案

* include 
貼上
* require
更強硬的引用，若出問題會報error並停止執行之後的程式
:::info
(xxx_once)
如果引用過，重複的引用會被忽略
:::

### ERROR顯示設定調整

#### 看設定
```php
phpinfo();
```
#### 路徑
```
/Applications/MAMP/bin/php/php7.4.2/conf/php.ini
```
#### 內容
```
display_errors = On (Xampp預設On MAMP預設Off)

error_reporting = E_ALL(都顯示)
=E_COMPILE_ERROR|E_RECOVERABLE_ERROR|E_ERROR|E_CORE_ERROR(只顯示ERROR)
```
* 在程式行前加上`@`，執行時會不顯示該行的錯誤訊息
* 在變數前加上`&`，**量子糾纏** (直接去取指標的值,call by reference)

### 運算子
#### 前後++差別 
```php
$x = $y++ // $x=$y,$y++ 先給值再加
$x = ++$y // $y++ ,$x=$y 先加再給值
```
#### && 跟 & 差別
```php
(條件a && 條件b) //條件a不符合 不執行條件b
(條件a & 條件b)  //條件a不符合 仍執行條件b
```


## 框架
laravel sympfony

## 參考資料
[php筆記](https://hackmd.io/@ETC/BJppieP8z)

哇今天好準時<3 可能老師餓了
