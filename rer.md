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

#結合telegram Bot

先到https://web.telegram.org 申請帳號

之後再左側搜尋框輸入`botFather`

然後跟他對話，一開始先輸入`/start`，然後輸入`/newbot`

再來輸入我們的bot名稱，以及bot帳號

>注意帳號不可重複，但有時雖然沒重複還是會出現錯誤

此時重新開啟telegram或是稍等一下再次輸入即可

成功後會出現類似如下，並給我們一個token

```
Done! Congratulations on your new bot. You will find it at t.me/kjjkbot. You can now add a description, about section and profile picture for your bot, see /help for a list of commands. By the way, when you've finished creating your cool bot, ping our Bot Support if you want a better username for it. Just make sure the bot is fully operational before you do this.

Use this token to access the HTTP API:.....
```

####使用Node.js與telegram bot溝通

之後我們使用`npm install node-telegram-bot-api --save`

然後貼上以下程式碼

```
var express = require('express');
var bodyParser = require('body-parser');
var util = require('util');
var app = express();
var port = 8000;

var TelegramBot = require('node-telegram-bot-api');
var token = '改為你的token';
//括號裡面的內容需要改為在第5步獲得的Token
var bot = new TelegramBot(token, {polling: true});



app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

app.post('/payload', (req, res) => {
  var obj = req.body.push.changes[0].new;
  msgPayload = `${obj.target.author.raw} push 到 ${obj.name} 分支，查看該次commit: ${obj.links.html.href}`
  bot.sendMessage(324090896, msgPayload); 
})

//Bot
bot.onText(/hihi/, function (msg) {
    var chatId = msg.chat.id; //房間ID
    var resp = '你好'; //括號裡面的為回應內容，可以隨意更改
    console.log(chatId);
    bot.sendMessage(chatId, resp); //發送訊息的function
});


app.listen(port,() => console.log(`listening on ${port}`));


```

記得先把token改為你的，然後執行程式

接著到telegram左上選單，點選`new group`，在輸入框輸入剛才的bot 帳號(username)

然後與這個bot對話，輸入`hihi`

看一下terminal的console，應該會出現一串數字，此為msg.chat.id

把它貼到上面`app.post('/payload')`中的`bot.sendMessage('', msgPayload); 
`

把第一個參數改為這串數字

最後試著push到bitbucket即可看到telegram出現訊息

![](/assets/螢幕快照 2017-02-05 下午12.57.53.png)