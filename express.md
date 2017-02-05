#Express框架



回顧HTTP章節，我們要創建server的過程較為繁瑣，我們可以使用相關的http server框架，這裡我們介紹Express

對歷史部分有興趣的同學

可參考:https://contentparty.org/r/3c6e5d38f358d553ac552fa7550e5f3e

##實作


# 使用express


```
npm init
```

```
npm install express --save
```
在test1.js內寫入
```
var express = require('express');
var app = express();
var port = 8000;

app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/

app.listen(port,() => console.log(`listening on ${port}`));
```
##但下面這是什麼?
```
app.use(express.static(__dirname + '/public'));
```
##試著把你剛test1.js複製一個到public資料夾，之後在網址打上http://localhost:8000/test1.js


可以設定多個靜態目錄
```
app.use(express.static('public'));
app.use(express.static('img'));
app.use(express.static('pdf'));
```

4.我們在test1.js 內加入下面，再執行看看
```
app.get('/', function (req, res) {
  res.send('Hello world!');
});
app.get('/hi', function (req, res) {
  res.send('hihi!');
});
```
##每次修改完都要重新啟動server覺得很麻煩

所以我們要安裝一個套件:forever

```
npm install forever -g
```
開始監控
```
forever --watch test1.js
```
結束監控
```
開啟另一個terminal 輸入 forever stopall
```
如果使用forever發現無法下指令關掉，可直接從os的工作管理員結束掉node.exe的程序即可
----------------------
5.接著我們加入更多路由功能
```
app.get('/', function (req, res) {
  res.send('Hello wogd!');
});
app.get('/hi', function (req, res) {
  res.send('Hiiii!');
});
 app.get('/', function (req, res) {
    res.send('Hello world');
  });
  app.get('/customer', function(req, res){
    res.send('customer page');
  });
  app.get('/admin', function(req, res){
    res.send('admin page');
  });
```
但發現這樣會很雜亂，所以我們新建一個目錄叫routes

裡面放入一個檔案index.js
```
module.exports = function (app) {
app.get('/', function (req, res) {
  res.send('Hello wogd!');
});
app.get('/hi', function (req, res) {
  res.send('Hiiii!');
});
 app.get('/', function (req, res) {
    res.send('Hello world');
  });
  app.get('/customer', function(req, res){
    res.send('customer page');
  });
  app.get('/admin', function(req, res){
    res.send('admin page');
  });

};
```
而 原本的檔案改為
```
var express = require('express');
var app = express();
var port = 8000;

var router = require('./routes/index.js')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/


app.listen(port,() => console.log(`listening on ${port}`));
```
這時我們試著把
```
var router = require('./routes/index.js')(app);
改為
var router = require('./routes')(app);

```
發現還是可以，原因是require如果指定為資料夾，他會預設去找下面的index檔案

express 是一個架構在http上的框架

##模板引擎

用於讓頁面與data分離的方式，在這裡我們要展示server side render data所以使用`EJS`當我們的範例

```
npm install ejs --save
```



>如果出現can't find ejs 重新輸入`npm install express`或是`npm install ejs -g`即可，原因:引入ejs是express源碼寫的`this.engine = engines[ext] || (engines[ext] = require(ext.slice(1)).__express);`所以他只會找和express同層目錄下的ejs

接著新增views資料夾
裡面放入test1.ejs
```
<!DOCTYPE html>
<html>
<head>

</head>
<body>
  <h1><%= title %></h1>
  <h1><%= message %></h1>
</body>
</html>

```

test1.js改為
```
var express = require('express');
var app = express();
var port = 8000;

app.set('view engine', 'ejs');
var router = require('./routes')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/


app.listen(port,() => console.log(`listening on ${port}`));
```

routes裡面的index.js改為
```
module.exports = function (app) {
  app.get('/', function (req, res) {
    res.render('test1', { title: 'Hello', message: 'Hello there!'});
  });
};
```

res.render後第二個參數物件為我們給到ejs上的變數

之後ejs使用`<%= ... %>`渲染然後轉為html傳給client


##app.locals設定全局變數

除了用script設定變數外，我們還可用app.locals或res.locals設定變數

app.locals可給所有render後的views使用

而res.locals只有當次request生效
```
app.locals = {
 test: 'Extended Express Example'
};
```

之後再test.ejs加上
```
 <h1><%= test %></h1>
```

後面的部分直接看專案的test1.js(由於目前gitbook文章內容過長，輸入非常Lag)