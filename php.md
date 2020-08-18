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

0與"0" => FALSE <br>
1 與"1" => TRUE


#### TimeStamp（時間戳記）
```php
<?= date("Y-y-M-m-D-d"); ?>
//2020-20-Aug-08-Mon-17

```

#### array
定義方法：
```php
$bloodType[] = 'A';
$bloodType[] = 'B'; 
或
$bloodType = array('A', 'B');
或
$bloodType = ['A','B'];

$變數名稱[ num ] = value
$變數名稱 = array('key' => value)
$變數名稱['key'] = value //key為字串

ex: $myArray['myName'] = 'Jeremy' 或 $myArray = array('myName'=>'Jeremy');
    echo "Hello! My name is ".$myArray['myName']
//輸出=>Hello! My name is Jeremy;

可使用var_dump 或 print_r 檢視陣列或物件內容
```
#### foreach
foreach( 陣列 as 變數名稱 )
將陣列內容一個一個讀進變數名稱

```php
foreach ($season as $key => $value){
	echo $key, "=>", $value;
}
```

#### 陣列排序

| 指令     | 意義                         |
| -------- | --------------------------- |
| sort()   | 以升序對索引陣列排序           |
| rsort()  | 以降序對索引陣列排序           |
| asort()  | 根據值，以升序對關聯陣列排序     |
| ksort()  | 根據鍵，以升序對關聯陣列排序     |
| arsort() | 根據值，以降序對關聯陣列排序     |
| krsort() | 根據鍵，以降序對關聯陣列排序     |
| natsort  | 自然順序算法對給定數組中的元素排序|

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

***
#### session
`session_start()`<br>
如果要使用session都必須要有這句做啟動

session Data存於伺服器端，使用者透過提交session ID讓server端提取對應的資料

#### cookies
cookies存於用戶端，可用於存放session ID.帳密等資料<br>
缺點是容易被竄改

[session vs cookies](https://medium.com/tsungs-blog/day14-session與cookie差別-eb7b4035a382)
#### exit
`exit($status)`<br>
中止腳本的執行，如果沒有status參數要傳入，可以省略括號
```php
header("Location: $url")
exit;
```
[exit](https://www.php.net/exit)

#### 字串處理

* `strlen()` 字串長度，在utf8編碼下中文字佔三個長度 
    * `mb_strlen( string, "string encoding" )` 加上編碼判斷 

<br>

* `strpos($字串,x,y)`從第一個y開始尋找x(y沒定義就是從頭找)
```php
$s="012345671289";
//    ^從這個往後找
$pos=strpos($s,"12",2); //8
--
$pos=strpos($s,"xxx"); //false 
找不到值會傳回 boolean   //(echo gettype $(pos))
```
如果查詢到某值的位置為０,該值會被php判斷成false，解決辦法：`!== false`
```php
$s = "012345671289";
    $pos = strpos($s,"012");
    
    if($pos !== false){
        echo("found: $pos"); //found:0
    }
    
    else{
        echo("Not Found");
    }
```
[查詢字符串第一次出現的位置](https://stockwfj3.pixnet.net/blog/post/66773997) 

<br>

* `substr($字串,x,y)`從第x個字取y個
```php
$s="01234567";
$result=substr($s,3,4);
echo $result;
//3456
```
* `str_reaplace("x","y",$字串)` 把字串裡的x替換成y
```php
$s="01234567";
$result=str_replace("12","-",$s);
echo $result;
//0-34567
```

### function
* `func_get_args()` 可變長度參數的函數
* 以字串間接呼叫function
```php
$p = "test";
echo $p($x); //相當於呼叫 test($x)函數;
```
* function無法讀到function以外的全域變數，除非使用 global
* 如果在function內使用未宣告的全域變數，會自動產生該全域變數，其他的function必須使用global才能使用該變數




```php=
$a = 20;

function myfunction($b) {
    print "a=$a"; // '',$a於function內未被定義
	$a = 30;
	print "a=$a"; // 30
	global $a, $c;// $c在這裡被定義為全域變數
	print "a=$a"; // 20
	return $c = ($b + $a);
}
print myfunction(40) + $c; //60+60=120
```


## 框架
laravel sympfony

## 參考資料
[php筆記](https://hackmd.io/@ETC/BJppieP8z)