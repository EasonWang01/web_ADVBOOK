全名為Read-Eval-Print-Loop (REPL)

主要用來產生一個terminal終端機程式，可接受使用者從終端機的輸入做對應的輸出，或是直接將想顯示的文字輸出至終端機


一開始我們先開期terminal，然後輸入node，進入nodejs命令列

之後輸入`.help`

1.試著輸入`.editor`

然後打上如下程式

```
function welcome() {
  return `Hello!`;
}

welcome();
```
接著按下鍵盤的ctrl + D

即可看到Hello出現在terminal上

2.試著輸入 `.save ./test2.js`

發現剛才的程式備存成檔案了

3.再來可輸入  `.load ./test2.js`
可用來讀取並執行一個js檔案

4.點選鍵盤左上的`tab`按鈕可列出所有變數


最後輸入兩次ctrl + C 即可退出nodejs命令列
