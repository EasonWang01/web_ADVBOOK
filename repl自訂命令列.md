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

2.接著看到範例程式部分