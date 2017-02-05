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

之後執行後終端機會產生兩組url分別是http與https的，我們把他加入bitbucket的webhook，並且在產生的url路徑後面加入`/payload`因為我們之後server要接post的路徑是`/payload`

![](/assets/螢幕快照 2017-02-05 上午11.38.42.png)

然後啟動我們剛才寫的server 之後隨意push到bitbucket該repository上即可看到server的log

>注意:如果ngrok重新啟動都會產生不同的url所以webhook的url也要更改

如果你是用EC2等虛擬主機或其他public IP則可以直接放入你的ip地址可以不用使用ngrok的服務



因為剛才的程式碼每次hook post過來的東西會包含以前舊的commit但我們只要新的，所以可改為

```


var express = require('express');
var bodyParser = require('body-parser');
var util = require('util');
var app = express();
var port = 8000;
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

app.post('/payload', (req, res) => {
 console.log(util.inspect(req.body.push.changes[0].new, {depth: null}));
})


app.listen(port,() => console.log(`listening on ${port}`));


```

之後把終端機的object複製到瀏覽器的devtool中，可以比較方便去觀察

![](/assets/螢幕快照 2017-02-05 下午12.09.21.png)