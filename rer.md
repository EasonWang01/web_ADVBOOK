首先我們要讓bitbucket有人push時可以通知我們的server


寫上如下程式在test1.js
```

var express = require('express');
var bodyParser = require('body-parser');
var app = express();
var port = 8000;
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

app.post('/payload', (req, res) => {
  console.log(req.body);
})


app.listen(port,() => console.log(`listening on ${port}`));
```

之後因為我們目前用`localhost`測試，所以我們架的server如果是在NAT或是firewall下面的話外面沒辦法使用post打入資料，所以我們要用`ngrok`https://ngrok.com/download這個軟體來讓我們server expose出去給外界

下載好之後解壓縮，之後在他的執行檔下面的同層目錄執行 `./ngrok http 8000` 8000為要開放給外界的port

ex:
```
~/downloads/ngrok http 8000
```