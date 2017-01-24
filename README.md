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

6.輸入npm init 並查看package.json

7.試著安裝一個套件 npm install express --save

8.有了package.json後輸入npm install 會自動安裝上面所寫出的套件

其他指令

```
npm install --save
npm install --save-dev //記得save跟dev要用-連再一起
npm install -g
npm uninstall
npm search
npm ls -g
npm ls -gl
npm ls -l
npm update -g
npm update
```

npm install  -g (使用後可在cmd的任何路徑輸入package名稱執行，但如果是想在js檔內直接使用require的話，要再把環境變數加上才行)(如此即可不用在每個專案資料夾個別安裝package))
(記得名稱要是NODE_PATH)

![](/assets/5a1c897c-0ff0-4f35-aa1c-36db81de39b6.png)
所以共有兩個環境變數:
一個是node_modules  =>給require用

一個是`C:\Users\Jason\AppData\Roaming\npm`給在cmd直接輸入module名稱用




