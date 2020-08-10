# JS

## Variables

### Class 類別

宣告一個函式
定義物件屬性及方法的藍圖

```javascript
Class 類別名稱
{
   放入屬性及方法
}
```


可使用extends繼承類別
```javascript
Class 類別名稱2 extends 類別名稱1{}
```

### Const
`const` 宣告的常數不可變更也不得為空，且無法重複宣告

陣列因為是物件參考(reference type)<br>指向的記憶體位置不能變，但是內容可變更


*錯誤範例：*
```javascript
const A = ["A","B","C","D"];
      A = ["A","B","C1","D"];
會error 無法使用同名物件去給新的值(記憶體位置衝突)
```
*正確使用：*
```javascript
const A = ["A","B","C","D"];
      A [2] = "C1";
會變成 A = ["A","B","C1","D"];
```

### Var vs Let 


| var      | let        |
| -------- | --------   |
| 可重複宣告 | 不能重複宣告 |
| 範圍在function內|範圍在區塊(if/for..)內|
| 初始值undefined|沒給值會error |


```javascript
var n; //undefined
var m = 1;
```

省略var會被認定為全域變數，可用window這個變數來取得最上層的全域物件
> window.variable

:::info
盡量使用Let來宣告變數
:::

---
## Operators

### \

後的符號可以跳離原先的身份 變成單純地表示用符號
```javascript
ex:3'14"
答案："3\'14\""
ex2:c:\doc\faq.txt
"c:\\doc\\faq.txt"
```

### 十進位轉二進位小數加減

0.1+0.2=0.30000000000000004 
(因為2進位10進位換算,2進位小數有問題）<br>
利用*10000(變成整數)之後再做運算 最後/10000來解決

---
## 條件陳述
JavaScript 提供 if 結構和 switch 結構，完成條件判斷，即只有滿足預設的條件，才會執行相應的陳述句。
### if 
先判斷一個運算式的布林值，然後根據布林值的真偽，執行不同的陳述句。
所謂布林值，指的是JavaScript 的兩個特殊值 true 表示真，false 表示 假。
```javascript
if (條件)
  陳述句;

// 或者
if (條件) 陳述句;
```
### if…else else if
if 代碼塊後面，還可以跟一個 else 代碼塊，表示不滿足條件時，所要執行的代碼。
```javascript
if (條件1) {
  // 滿足條件1時，執行的陳述句
} 
else if(條件2){
 // 滿足條件2時，執行的陳述句
} 
else {
  // 都不滿足條件時，執行的陳述句
}
```

### switch
多個 if...else 連在一起使用的時候，可以轉為使用更方便的 switch 結構。
```javascript
switch (fruit) {
  case "banana":
    // ...
    break;
  case "apple":
    // ...
    break;
  default:
    // ...
}
```
### 三元運算符
JavaScript 還有一個三元運算符（即該運算符需要三個運算子）?:，也可以用於邏輯判斷。
```javascript
(條件) ? 運算式1 : 運算式2
ex.
var even = (n % 2 === 0) ? true : false;
```

## 迴圈控制
### If vs Loop

* in & of
```javascript
var datalist=[10,20,30];

for(let i = 0; i < dataList.length; i++) //陣列長度
for(data of datalist) //抓出陣列的內容 (10,20,30)
for(data in datalist) //抓出陣列的編號 (0,1,2)
```
`in` 會自動跳過陣列內的空值而 `of` 不會，需加入以下片段來判斷
```javascript
if (typeof data === "undefined") {
    continue;
}
```

### break vs continue
`break` 和 `continue` 都具有跳轉作用，可以讓代碼不按既有的順序執行
* `break` 陳述句用於跳出代碼塊或迴圈(while/if..)
```javascript
var i = 0;

while(i < 100) {
    console.log(i);
    i++;
    if (i === 10) break;
} // <-- 跳到這裡
```
上面只會執行10次迴圈，一旦 i 等於 10，就會跳出迴圈

<br>

* `continue` 陳述句用於跳過本輪迴圈<br>傳至迴圈結構的頭，繼續下一輪迴圈
```javascript
var i = 0;

//  --> 跳到這裡
while (i < 100){
    i++;
    if (i % 2 === 0) continue;
    console.log(i);
}
```
上面只有在 i 為奇數時，才會輸出 i 的值；
如果 i 為偶數，則直接進入下一輪迴圈

如果存在多重迴圈，不帶參數的 `break`和 `continue` 陳述句都只針對最內層迴圈

## 等於
= : 傳值

== : 值一樣就好

=== : 值和型態都要一樣

undefined == "undefined", 0, NULL, 空集合

undefined === "undefined"

NaN不等於任何東西，包含自己(NaN != NaN)

---
## function
```javascript
function 函數名稱 （參數1,參數2）{} //參數用逗號分隔
function 函數名稱 （參數1,參數2 = 預設值）{} //有預設值的置於後方
```
## Math

Math.random() 隨機0-1的數字

Math.round() 四捨五入

Math.floor() 取小於這個數的最大整數(取高斯)

Math.ceil() 取大於這個數的最小整數(取上高斯)

```javascript
ex:
return Math.floor(Math.random() * img.length);
回傳陣列長度內的隨機圖片
```


## 作業

寫網頁
截圖對照網頁&程式的變化

## 參考資料

[js筆記](https://github.com/dongshaohan/jsnote#written)

[js筆記](https://hackmd.io/zZe_oRgfQp6YFboDCVAvpg)

[過橋遊戲](http://www.plastelina.net/game3.html)


### 💰老師語錄
#### 你不這樣麻煩你會更麻煩
#### 雖然他死了 但是他沒有死(秦始皇)
#### 是吧 對齁? 對
#### 你半小時計算一下妳幹了啥 還敢打小秘境啊
