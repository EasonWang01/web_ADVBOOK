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

所以第一步先把server side 的code 用到es6的先轉好，才不會出錯


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
    app:[
    './src/client/client.js'
  ],
},

  output: {
    path: require("path").resolve("./src/dist"),
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

然後把server.js用到webpack的也拿掉

之後把bundle.js放到`express.static`的目錄下即可



##4.設定nginx reverse proxy



##5.加入https，使用Let's encrypt