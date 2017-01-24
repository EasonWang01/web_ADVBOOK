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

>有兩種scope範圍，也就是套件可以安裝在全域，也可以安裝在特定資料夾


3.而你也可以把你自己的套件打包好放到NPM上讓其他人下載

4.搜尋別人寫的package https://www.npmjs.com/package/react

5.開啟terminal，輸入npm查看相關指令  

6.輸入npm init 並查看package.json

7.試著安裝一個套件 npm install express --save

8.有了package.json後輸入npm install 會自動安裝上面所寫出的套件
```
試著在package.json中加入如下

"prore": "^1.1.0",
```
然後輸入npm install



npm install  -g (使用後可在cmd的任何路徑輸入package名稱執行，但如果是想在js檔內直接使用require的話，要再把環境變數加上才行)(如此即可不用在每個專案資料夾個別安裝package))
(記得名稱要是NODE_PATH)

![](/assets/5a1c897c-0ff0-4f35-aa1c-36db81de39b6.png)
所以共有兩個環境變數:
一個是node_modules  =>給require用

一個是`C:\Users\Jason\AppData\Roaming\npm`給在cmd直接輸入module名稱用


##為了避免部屬後環境module過大，可不必安裝dev用的module
一開始開發時將套件安裝到devDependencies
```
npm install --save-dev //記得save跟dev要用-連再一起

```
部屬時不安裝devDependencies的模組則輸入
```
npm install --production
```


>當npm install出現一些版本錯誤，而無法安裝，這是記得先更新本地端`npm install -g`(更新global的package)
更多可參考
https://docs.npmjs.com/


#其他指令

```
npm uninstall  //解除安裝套件，例如：npm uninstall express -g
npm search   //搜尋套件 例如：npm uninstall express -g
npm ls -g  //列出所有全局套件
npm ls -gl  //列出全域套件詳細資訊
npm ls -l  //列出專案裡的套件詳細資訊
npm update -g  //更新全域套件
npm update  //更新專案裡的套件
```



#package.json教學

1.
`"main"`表示require('模組名稱')所預設加載的文件。

2.如下的寫法可用`npm run start`輸入此即會執行`node index.js`

```
"scripts": {
"start": "node index.js"
},
```
3.
config用來設定環境變量，如下
```
"config": { "port" : "8080" }
```

可在程式中使用
`process.env.npm_package_config_port`讀取到

比較常用設定環境變量的方法為
```
console.log(process.env.PORT)
```
然後執行
```
PORT=8000 node test1.js //mac
node test1.js PORT=8000 //windows
```



