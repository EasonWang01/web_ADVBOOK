# #部署

我們接下來要把前幾章實作的網站部署到AWS上，並用nginx當proxy


## 1.使用AWS

AWS 的free Tire有12個月的免費方案，我們將使用此方案

https://aws.amazon.com/

1.之後點選右上角的`sign in`

如果沒有帳號可以先註冊，但他會要求你先填入信用卡帳號，之後他會刷一筆小金額測試，但不會請款

2.登入後先確認右上角的地區是離你最近的地區，之後開機器會在離你近的地區，連線速度會比較快

3.點選左上方的Services選單之後選擇EC2

4.點選Launch Instance

5.我們這裡選擇Ubuntu Server的選項，之後點選右下方next，可把容量設為30G，然後security Group 開啟80 PORT

6.創建新的Key，之後使用此private key連線
>如果告知WARNING: UNPROTECTED PRIVATE KEY FILE!，可使用
chmod 0600 [private key file] 即可

7.之後開啟terminal使用ssh連線，指令類似如下

```
ssh -i ~/Downloads/pem1.pem  ubuntu@ec2-13-112-175-93.ap-northeast-1.compute.amazonaws.com
```

第一次登入需輸入`yes`



##2.安裝nginx與Node.js

```
sudo apt-get install nginx 
```
之後到我們server 的ip即可看到畫面
ex:`http://13.112.175.93/`


之後安裝node.js

```
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
```
然後
```
sudo apt install -y nodejs
```



##3.使用babel與webpack轉碼之後啟動Node.js server 使用pm2模組

首先我們輸入
```
git clone https://Easonwang01@bitbucket.org/Easonwang01/node.js-with-react.js_.git
```
之後cd 到目錄

因為我們要在nginx下再架一個nodejs server



所以先把server.js開發時用到的webpack部分拿掉
```
sudo vim ./src/server/server.js
```

server.js 刪掉下面幾行
```
var config = require('../../webpack.config.js');
var webpack = require('webpack');
var webpackDevMiddleware = require('webpack-dev-middleware');
var webpackHotMiddleware = require('webpack-hot-middleware');
var compiler = webpack(config);
app.use(webpackDevMiddleware(compiler, {noInfo:true,publicPath: config.output.publicPath}));
app.use(webpackHotMiddleware(compiler));
```



第二步是要把server side 的code 用到es6的先轉好，才不會出錯


先`sudo npm install babel-cli -g`

然後`npm install`

之後輸入

```
babel src -d dist --presets es2015,stage-2 --copy-files
```

可發現多出了一個dist資料夾，這個是把node.js的一些ES6以上code做轉碼


記得把hmr等plugin拿掉
```
sudo vim ./webpack.config.js
```
把檔案改為
```
var webpack = require('webpack');

module.exports = {
  entry: {
    app: './src/client/client.js'
},

  output: {
    path: require("path").resolve("./dist/client/"),
    filename: 'bundle.js',
    publicPath: '/'
  },
  plugins: [
    new webpack.optimize.OccurrenceOrderPlugin(),
  ],
  module: {
    loaders: [
      {
        test: /\.js$/,
        loader: 'babel',
        exclude: ['/node_modules/','/src/server/','/src/client',],
        query: {
          presets: ['react', 'es2015','stage-0'],
          cacheDirectory: true
        },
      },
      {
        test: /\.css$/,
        loader: 'style-loader!css-loader'
      }
    ]
  }
}

```


最後把client的code build一份bundle.js，在webpack.config.js同層使用

```
sudo npm install webpack -g
```
雖然有些建議不要用sudo安裝模組，但目前先這樣安裝沒關係
```
sudo webpack
```
上面的指令輸入完需等待約20秒，請先不要按退出




之後把bundle.js放到`express.static`的目錄下即可(這邊因為webpack輸出時已經放到dist中的client所以不用更動)

再來安裝pm2

```
sudo npm install pm2 -g
```

然後安裝Redis環境

```
sudo apt install redis-server
```

之後到aws 的 security group的 inbound 開啟3001 port

![](/assets/螢幕快照 2017-02-06 下午2.02.39.png)

>這時通常可直接執行Node.js程式然後到IP對應的port測試，但有時必須使用reverse proxy才能連到程式

##4.設定nginx reverse proxy

安裝

```
apt-get install nginx  
```

設定

```
 sudo vim /etc/nginx/sites-available/default
```

將location設為

```
location / {
    proxy_pass http://localhost:3001;
  }
```


然後重新啟動nginx

```
sudo nginx -s stop && sudo nginx
```

最後使用pm2執行程式

```
pm2 start ./dist/server/server.js
```

之後即可到我們的ip查看

http://13.112.175.93/

##5.加入https，使用Let's encrypt

首先我們要先申請一個域名

#### 域名設定

#### 1.以下用AWS EC2 與Goddy為例

致電godaddy後他們的客服都不太能解決問題，一開始我看網路上教導說使用AWS Route53服務，但注意，此不包含在Free tire 需另外收費，但還是稍微講解route53的dns設定，當初參考此篇[https://www.quora.com/How-do-I-route-my-GoDaddy-domain-name-to-my-Amazon-EC2-web-server的第一個回答，但他的回答還少了兩點，就是還要再route53加上兩筆A](https://www.quora.com/How-do-I-route-my-GoDaddy-domain-name-to-my-Amazon-EC2-web-server的第一個回答，但他的回答還少了兩點，就是還要再route53加上兩筆A) record 指向你的example.com和www.example.com，才算完成．

> 但要如何用免費的EC2不用route53指向我們在godaddy買的網域呢？

因為一開始EC2給你的都是浮動的IP也就是你把機器重開後IP位置就會改變，所以我們要先申請Elastic IP，一樣在EC2的console那可申請，之後你的EC2的ssh與public ip會自動改為你新申請好的Elastic IP之後再到Godaddy點選域名伺服器，但注意要選`使用預設名稱伺服器`，之後會發現上面多出一個框框  

之前一直很納悶為什麼都沒有出現這個，直到把`域名伺服器`選項從自訂改為預設後才出現，之後就把A記錄改為你的EC2 IP 然後再把CNAME 的 WWW改為 你的域名即可，  
即為`@`

![](/assets/螢幕快照 2017-02-06 下午3.52.194.png)

#使用Let's encrypt

1.先到此網站

https://www.sslforfree.com

(如果沒顯示，換個瀏覽器)

2.之後輸入你的網域名稱

3.點選Manual verfication

4.之後把它給你的兩個檔案下載，之後放到你的server www目錄下

建造一個資料夾`.well-known`然cd進入

再創一個`acme-challenge`

最後放入兩個下載的key


>1.Create a folder in your domain named ".well-known" if it does not already exist
>
2.Create another folder in your domain under ".well-known" named "acme-challenge" if it does not already exist

>3.Upload the downloaded files to the "acme-challenge" folder

ubuntu路徑如下

`/usr/share/nginx/`

之後點擊下面連結測試，如果有下載下來即為成功

之後就可以點下面綠色按鈕，往下一步

(如閒置太久點下一步時會產生錯誤，需重來)

###接著是重點

5.它會給你三個框框，裡面分別為
```
ca_bundle.crt 
private.key 
certificate.crt
```
用nginx為例子

我個人把它們存放再`/usr/share/nginx/sslcrt`，其他路徑也可以，只要後面的nginx default 檔案內設定相同即可

另外要輸入以下指令，將兩個crt合成一個bundle
```
sudo bash -c 'cat certificate.crt ca_bundle.crt >> bundle.crt'
```

我們要在nginx的default設定檔案內設定https


ubuntu路徑如下

`/etc/nginx/sites-available`

會有一個default檔案，用vim等文字編輯器開啟


開啟後如前面沒設定過，會都是藍色的註解

把它更改如下圖(如果說private.key not match ，把ssl_certificate 的bundle.crt改為certificate.crt試試)






之後

```
1.sudo service nginx stop

2.sudo nginx
```
###注意

這裡可能出現一些key或bundle的https錯誤，最常見的是說begin或end之類，記得每個檔案要有
```
-----BEGIN CERTIFICATE-----

....

-----END CERTIFICATE-----
```
這不是註解

##最後在你的網域前加上`https://`即可


參考至:https://free.com.tw/ssl-for-free/


#完整nginx config

完整範例:
```
server {        
  listen 80;        
  server_name sakatu.com  www.sakatu.com;
  rewrite ^/(.*) https://sakatu.com/$1 permanent;
}
server {        
  server_name sakatu.com;
  listen 443;
  ssl on;
  ssl_certificate  /usr/share/nginx/sslcrt/bundle.crt;        
  ssl_certificate_key /usr/share/nginx/sslcrt/private.key;     
  location / {          
    proxy_pass http://localhost:3000;      
  }}
```



