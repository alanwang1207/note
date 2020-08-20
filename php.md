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

| 指令      | 意義                                           |
| -------- | ---------------------------                    |
| sort()   | 以升序對索引陣列排序(字串排列)                      |
| rsort()  | 以升序對索引陣列排序                              |
| usort()  | 以自訂function排序陣列值 usort($array,"funName")   | 
| asort()  | 根據值，以升序對關聯陣列排序                        |
| ksort()  | 根據鍵，以升序對關聯陣列排序                        |
| arsort() | 根據值，以降序對關聯陣列排序                        |
| krsort() | 根據鍵，以降序對關聯陣列排序                        |
| natsort  | 自然順序算法對給定數組中的元素排序                   |

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
[session vs cookie](https://ithelp.ithome.com.tw/articles/10157724)
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

* `htmlspecialchars($string)` 將字串中HTML符號變成不可執行的文字符 <資安

### function
* `func_get_args()` 可變長度參數的函數
* 以字串間接呼叫function
```php
$p = "test";
echo $p($x); //相當於呼叫 test($x)函數;
```
* function無法讀到function以外的全域變數，除非使用 global
* 如果在function內使用未宣告的全域變數，會自動產生該全域變數，其他的function必須使用global才能使用該變數

php中的方法及類別不分大小寫


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

### 讀寫檔案

* opendir：打開一個目錄
* readdir：返回由opendir() 打開的目錄中的文件名稱
* closedir：關閉由 opendir() 函數打開的目錄
* file_put_contents：快速寫入檔案
* move_uploaded_file：將上傳的檔案移動到新位置
* fopen：打開文件或者URL
* fread：讀取文件
* fwrite/fputs：寫入文件
* fgets：從文件指針中讀取一行
* htmlspecialchars:轉換HTML 特殊符號為僅能顯示用的編碼

### Class
```php
Class Animal{
        public $price;        
        private $_weight;
}
```

#### __construct(建構子) 
一旦宣告後就直接執行,經常會用來執行必要動作

#### __destruct(解構子)
這個類別執行結束之後再執行 __destruct 內的動作,經常拿來執行收尾的工作

[interface和implements](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/242253/)

### 資料庫
#### 連接資料庫 
```php
$link = mysqli_connect("host","user","password","database","port","socket")
or die(mysqli_connect_error());

mysqli_query($link, "set names utf8");
mysqli_select_db($link, "資料庫");

//可在mysql_connect前加＠可用來停用錯誤訊息
```
#### 關閉連接
```php
mysqli_close($link)
```
#### 選取資料表資料
```php
$sql = "select * from 資料表";
$result = mysqli_query($link, $sql);
```
#### 顯示查詢結果
```php
mysqli_fetch_assoc($result) //使用資料庫欄位取值
echo "ID：{$row['cID']}";

mysqli_fetch_row($result)   //使用數字索引取值
echo "ID：{$row[0]}";

mysqli_fetch_array()

mysqli_fetch_object()

```
#### 查詢筆數
```php
mysql_num_rows($result)
```


### MVC架構
* model：對資料庫的存取，異動，檢查
* view:視圖，前端畫面
* controller：路由，流程控制
![](https://i.imgur.com/aD6K8ZG.png)

### .htaccess (路由設定檔)
```
RewriteEngine on
//找尋檔案中的file跟directory 
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
//找不到的由瀏覽器來處理
RewriteRule ^(.*)$ index.php?url=$1 [QSA,L]
```
.htaccess跟Apache共同工作完成一次路徑的解析

### curl

* curl_init() 建立curl連線
* curl_setopt() 設定擷取的url網址
* curl_exec()
* curl_close() 關閉curl連線


```php
$_SERVER['HTTP_HOST']：當前請求的Host頭中的內容(與取得Server的Port)
$_SERVER['SERVER_NAME']：當前運行網頁檔案所在的主機名稱
$_SERVER['REQUEST_URI']：訪問此頁面需要的URL
$_SERVER['PHP_SELF']：當前正在執行的網頁檔案名稱
$_SERVER['QUERY_STRING']：查詢的變數值
```



# 作業

會員系統(session)：
首頁 會員頁 登入頁<br> 
登入 登出 

trim() 清除字串前後空白

電特多做
1.MVC版本 
2.laravel版本
3.Lab_REST_API 全部做一遍 截圖

<hr>

## 參考資料
[php筆記](https://hackmd.io/@ETC/BJppieP8z)

[兔子吃肉](https://kknews.cc/zh-tw/news/q4qjy3o.html)

[filesystem函數](https://www.w3school.com.cn/php/php_ref_filesystem.asp)



## 框架(laravel sympfony)


拉for
卑鄙源之助
轟隆隆隆隆隆隆隆隆隆隆隆隆 衝衝衝衝

引擎發動

拉風

我是今夜最稀有品種

重點是那三個女生不會跟我在一起

哈利波特經典鮑

我還沒看哈利波特

浪漫DUKE 帶你找回屬於你的浪漫 浪漫突進

ありがとう　神様

熟人都叫我 東太平洋 漁場時價分析師 兼操盤手

暨洋流講師 海龍王彼得

邱喔




