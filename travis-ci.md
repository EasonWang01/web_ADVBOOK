# #自動化流程與搭建travis ci

我們先講有關測試部分

1.新增一個資料夾

2.terminal cd 進入該資料夾 ，執行`npm init`

3.安裝`mocha`與`should`兩個模組

>注意:mocha要安裝在 global

```
npm install mocha -g             
npm install mocha --save-dev     
npm install should --save-dev
```

should的API可參考
http://shouldjs.github.io/#assertion-above

我們新增一個index.js

```
exports.test1 = (num) => {
  return 10 + num;
}
```

之後新增test資料夾

