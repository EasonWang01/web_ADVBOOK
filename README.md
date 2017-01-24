# \#1.安裝Node.js

https://nodejs.org/en/download/







# \#2.測試是否安裝成功

1.開啟terminal 輸入node



2.使用sublime，atom或任何編輯器創造如下檔案

```
class.js


function hello() {
  console.log('Hello Hello!');
}
hello();

```

之後開啟終端機，並輸入`cd 加上你的檔案路徑`

輸入node class

# #2.使用NPM

1.安裝Node.js時自動會安裝NPM

2.他是一個套件管理工具，類似一個倉庫，我們可以從裡面去下載東西出來用

3.而你也可以把你自己的套件打包好放到NPM上讓其他人下載

4.搜尋別人寫的package https://www.npmjs.com/package/react

5.開啟terminal，輸入npm查看相關指令  

6.試著安裝一個套件 npm install express -g

7.輸入npm init 並查看package.json

8.有了package.json後輸入npm install 會自動安裝上面所寫出的套件

其他指令

```
npm install --save
npm uninstall
npm search
npm ls -g
npm ls -gl
npm ls -l
npm update -g
npm update
```




