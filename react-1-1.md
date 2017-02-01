# 開始使用React 搭配 Webpack

前一章我們使用codepen與babel的cdn來幫我們compile我們寫的ES6與React的JSX語法，但我們開發時通常會使用webpack結合babel與一些plugin來幫我們做compile

## 建立環境

`npm install webpack -g`

`npm install nodemon -g`
\(在更改程式時自動執行server，與forever類似，但forever遇到錯誤也不會停止\)

1.裡面放入package.json

```
{
    "name": "react-learning",
    "version": "1.0.0",
    "description": "A simple app",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "serve": "nodemon server/server.js"
    },
    "repository": {
        "type": "git",
        "url": ""
    },
    "author": "",
    "license": "ISC",
    "dependencies": {
        "babel-core": "*",
        "babel-loader": "*",
        "babel-preset-es2015": "*",
        "babel-preset-react": "*",
        "express": "*",
        "react": "*",
        "react-dom": "*",
        "webpack": "*"
    }
}
```

之後輸入`npm install`

在根目錄下新建三個目錄

```
  --client
  --components
  --server
package.json
```

2.接著在server目錄下新增server.js

```
var express = require('express');
var path = require('path');

var app = express();

app.use(express.static('./dist'));

app.use('/', function (req, res) {
  res.sendFile(path.resolve('client/index.html'));
});

var port = 3000;

app.listen(port, function(error) {
  if (error) throw error;
  console.log("Express server listening on port", port);
});
```

3.在client資料夾內加入index.html

```
<!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
    <title>React example</title>
    </head>
    <body>
      <h1>hihi !</h1>
      <div id="app"></div>
      <script src="bundle.js"></script>
    </body>
</html>
```

4.新增webpack 配置文件webpack.config.js

```
module.exports = {
  devtool: 'inline-source-map',
  entry: ['./client/client.js'],
  output: {
    path: './dist',
    filename: 'bundle.js',
    publicPath: '/'
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/,
        query: {
          presets: ['react', 'es2015']
        }
      }
    ]
  }
}
```

什麼是source map

可以看chrome dev tool 裡的setting即有此選項
[http://www.ruanyifeng.com/blog/2013/01/javascript\_source\_map.html](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html)

5.在client資料夾中新增client.js

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from '../components/App';

ReactDOM.render(
<App/>,document.getElementById('app')
)
```

`<App/>`即為我們的react元件

6.在components資料夾中新增App.js

此即為我們第一個react元件

```
import React, { Component } from 'react'

class App extends Component {
  render() {
    return <div>I'm Banana!</div>
  }
}

export default App
```

之後輸入`webpack --config webpack.config.js`
會自動產生dist資料夾，裡面包含bundle.js檔案，此為webpack打包後的東西

之後於terminal輸入，`npm run serve`\(寫在package.json中的scripts內\)

執行伺服器，並打開瀏覽器輸入開啟`localhost:3000`


####下載React devtool
https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi

之後chrome devtool的tab會多一個`React`

## 讓我們不用重新整理網頁

在package.json內加入

1.不用重新整理網頁
\(讓我們不用使用webpack-dev-server也有-hot的指令\)

```
"webpack-hot-middleware": "*"
```

2.讓hot middleware知道react的class

```
"babel-preset-react-hmre": "*",
```

以及讓webpack跑在我們架設的express server上

```
"webpack-dev-middleware": "*"
```

使用指令 

```
npm install --save webpack-hot-middleware babel-preset-react-hmre webpack-dev-middleware
```

來安裝以上三個package


接著更改剛才server資料夾下的 server.js

```
var express = require('express');
var path = require('path');
var config = require('../webpack.config.js');
var webpack = require('webpack');
var webpackDevMiddleware = require('webpack-dev-middleware');
var webpackHotMiddleware = require('webpack-hot-middleware');

var app = express();

var compiler = webpack(config);

app.use(webpackDevMiddleware(compiler, {noInfo: true, publicPath: config.output.publicPath}));
app.use(webpackHotMiddleware(compiler));

app.use(express.static('./dist'));

app.use('/', function (req, res) {
res.sendFile(path.resolve('client/index.html'));
});

var port = 3000;

app.listen(port, function(error) {
if (error) throw error;
console.log("Express server listening on port", port);
});
```

現在我們可以直接用server.js去compile 我們的webpack config檔案，不用再輸入指令compile

最後因為我們剛才有用hot middle所以我們可以使用--hot去讓他自動reload網頁，但我們不想在指令輸入，所以可以把他加在webpack config內