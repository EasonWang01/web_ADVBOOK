# #Webpack

目前有出第二版也就是webpack2，但目前性能與社群比較起來還是暫時先用一會比較適當，所以以下教學將以官方教學介紹webpack

# 附錄0-Webpack

其官方敘述:
>A bundler for javascript and friends. Packs many modules into a few bundled assets. Code Splitting allows to load parts for the application on demand. Through "loaders" modules can be CommonJs, AMD, ES6 modules, CSS, Images, JSON, Coffeescript, LESS, ... and your custom stuff

以前用過Browserfy、gulp、grunt等工具的話可以迅速理解他的概念


#開始使用

打開終端機，輸入
`npm install webpack -g`
![](0245.png)

下面我們以官方的範例，讓大家了解webpack基本功能

###1.創造如下檔案

entry.js
```
document.write("It works.");
```
index.html
```
<html>
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <script type="text/javascript" src="bundle.js" charset="utf-8"></script>
    </body>
</html>
```
之後在終端機輸入`cd  你創建的資料夾`

(讓終端機路徑進入的你資料夾內)

接著輸入

` webpack ./entry.js bundle.js`

上面指令的用途為把你的entry.js檔案打包為bundle.js

成功輸入指令後會看到如下
```
Version: webpack 1.12.11
Time: 51ms
    Asset     Size  Chunks             Chunk Names
bundle.js  1.42 kB       0  [emitted]  main
chunk    {0} bundle.js (main) 28 bytes [rendered]
    [0] ./tutorials/getting-started/setup-compilation/entry.js 28 bytes {0} [built]
```

之後可以打開 index.html 查看

以上即是我們第一個用webpack打包的程式

###2.創建第二個js檔案
於同一個資料夾下新增content.js檔案
```
module.exports = "It works from content.js.";
```
再來，將原先entry.js改為
```
document.write(require("./content.js"));
```
之後一樣輸入於終端機輸入
```
 webpack ./entry.js bundle.js
```
打開index.html可以發現，畫面顯示出content.js的內容

>這個範例用途為，讓我們了解webpack的模組化js檔案功能

###3.使用Loader
因為webpack原生只能處理js檔案，所以想處理其他檔案時，我們必須安裝對應的Loader。

下面為css-loader的範例

我們先來安裝load，安裝方式為在終端機使用npm安裝
```
npm install css-loader style-loader
```
這裡沒有於結尾處加上`-g` 所以他會在我們的資料夾產生node_module資料夾，裡面放入我們用npm所安裝的東西 

安裝好後我們新增一個css檔案

style.css
```
body {
    background: yellow;
}
```
接著更新entry.js檔案
```
require("!style!css!./style.css");
document.write(require("./content.js"));
```
接著一樣輸入
```
 webpack ./entry.js bundle.js
```

但是

`require("!style!css!./style.css");`

太長

所以我們可以把它寫在command

將entry.js改為
```
require("./style.css");
document.write(require("./content.js"));
```
這次使用下面的指令compile
```
webpack ./entry.js bundle.js --module-bind 'css=style!css'
```

###4.webpack.config.js

通常許多模組都會有一個config file，而webpack也有

用來敘述我們在compile時要請webpack做什麼事

下面我們新增一個檔案，名稱為webpack.config.js

```
module.exports = {
    entry: "./entry.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style!css" }
        ]
    }
};
```
這時我們再compile一下，但只要輸入如下即可

```
webpack
```


>webpack會自動去搜尋目錄下的webpack.config.js內的配置

###5.增加compile時畫面的豐富性

可以使用以下指令compile試試，會增加一個進度條，與顏色
```
webpack --progress --colors
```

其他指令可輸入 webpack --help觀看

###6.自動compile
每當我們更改檔案後都要手動輸入compile指令，使用上較為麻煩，我們可以使用--watch，讓webpack發現檔案有改變時，自動幫我們compile
```
webpack --watch
```

###7.使用webpack dev server
我們先安裝，所以先在終端機輸入
```
npm install webpack-dev-server -g
```
之後輸入

```
webpack-dev-server --progress --colors
```

接著輸入網址
```
http://localhost:8080/webpack-dev-server/bundle
```

即可看到如下畫面

![](452.png)

試著改變content.js檔案內文字，並按下儲存，隨即瀏覽器畫面也會跟著更改


####如果想打包成多個檔案可以如下寫

```
var webpack = require("webpack");
module.exports = {
    entry: { a: "./a", b: "./b" },
    output: { filename: "[name].js" }
}
```


#有關webpack loader介紹

Loaders 意思簡單來說是，當webpack發現這些後綴名的檔案時，要用某種方式去解析它

可看如下範例

```
{
 當遇到名稱為.ts檔案時，將它解析為typescript
  test: /\.ts/,
  loader: 'typescript',
},
{
  // 當遇到圖片時， 使用 image-webpack 壓縮 
  // 並將他們轉為 data64 URLs
  test: /\.(png|jpg|svg)/,
  loaders: ['url', 'image-webpack'],
},
{
  //當遇到 SCSS 檔案時,使用 node-sass解析, 並使用 autoprefixer 
  //然後返回css的字串
  test: /\.scss/,
  loaders: ['css', 'autoprefixer', 'sass'],
},
{ //將後綴名為jsx使用jsx-loader解析
  test: /\.jsx$/,
  loader: 'jsx-loader'
},
{
    test:   /\.js/,
    loader: 'babel',
    include: __dirname + '/src',
    //include可指定只解析特定資料夾
},
{
  test: /\.woff$/,
  loader: 'url?limit=50000',
  include: PATHS.fonts
},

{
    loader: "babel-loader",//和babel相同
    exclude: /(node_modules|bower_components)/,
    test: /\.jsx?$/,
    query: {//寫query我們就不用另外寫.babelrc檔案
        plugins: ['transform-runtime'],
        presets: ['es2015', 'stage-0', 'react'],
      }
   //exclude可排除解析特定資料夾
  
```
最後所有經過解析的東西都會轉為字串，類似如下
```
export default 'body{font-size:12px}';
```

#有關Webpack resolve
```
  resolve: {
        //從下面路徑開始找尋module
        root: 'E:/github/src', 
        
        //文件後綴名宣告，之後使用require可省略後綴
        extensions: ['', '.js', '.json', '.scss'],
        
        //路徑別名，之後引用可直接使用別名
        alias: {
            AppStore : 'js/stores/AppStores.js',//直接 require('AppStore') 即可
            ActionType : 'js/actions/ActionType.js',
            AppAction : 'js/actions/AppAction.js'
        }
    }
```


#有關Webpack plugin介紹

####1.CommonsChunkPlugin
```
var webpack = require("webpack");

module.exports = {
  entry: {
    app: "./app.js",
    vendor: ["jquery", "underscore", ...],
  },
  output: {
    filename: "bundle.js"
  },
  plugins: [
    new webpack.optimize.CommonsChunkPlugin(/* chunkName= */"vendor", /* filename= */"vendor.bundle.js")
  ]
};
   // 第一個參數為entry 內的屬性名稱
   // 第二個參數是輸出檔案的名稱
```
所以之後會打包為兩個檔案，一個為原本寫在Output中的檔案，另一個即為上述vendors.bundle.js。

記得於entry中寫`vendor:[你要加入的module]`

並於html中引用

```
<script src="vendor.bundle.js"></script>
<script src="bundle.js"></script>
```

這樣做的好處是，分離第三方套件與專案內部程式碼。由於專案內部程式碼會不斷做更新，而第三方套件通常不會修改，如果沒有與第三方套件分開打包的話，使用者在每一次更新後都必須下載第三方套件加上專案內部程式碼的一整包檔案，使專案運行起來的速度緩慢。

####2.Extract-text-webpack-plugin 

CSS 被 require() 時，webpack 會自動生成一個 `<style>` 標籤並加入到 html 的 `<head>` 中，但我們有時不希望css一同被打包，而希望其產生.css之後再用`<link>`方式引入。

可類似如下寫出
```
    var webpack = require('webpack');
    var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');
    var ExtractTextPlugin = require("extract-text-webpack-plugin");

    module.exports = {
        plugins: [commonsPlugin, new ExtractTextPlugin("[name].css")],
        entry: {
```
####3.寫externals

用途也是解決打包後文件過大，載入變慢的問題，解法為將其隔離出bundle.js，而於index.html之內用script引用
```
module.exports = {
    externals: {
      'react': 'React' 
    },
    //...
}
```
index.html
```
<script src="react.min.js" />
<script src="bundle.js" />
```
如此來手動引用js檔案，記得要寫在bundle.js之前

####4.使用 DefinePlugin
```
module.exports = {
  //...
  plugins:[
    new webpack.DefinePlugin({
      'process.env':{
        'NODE_ENV': JSON.stringify('production')
      }
    }),
   
  ]
  //...
}
```
可以設定你的執行環境，當要真正產生production時, module會把一些在dev環境下的code check拿掉，來增加執行速度 

以React的 react.min.js 為範例. 在其 `./node_modules/react/lib`中, 會看到如 `process.env.NODE_ENV !== 'production'`的程式碼. 

當我們的環境是Production時，裡面的程式碼不會執行，所以可以增加速度





#webpack.config.js說明

```
module.loader: 其中test是正則表達式，對符合的文件名使用相應的加載器. 

/.css$/會匹配 xx.css文件，但是並不適用於xx.sass或者xx.css.zip文件.

url-loader 它會將樣式中引用到的圖片轉為模塊來處理; 配置信息的參數“?limit=8192”表示將所有小於8kb的圖片都轉為base64形式。

entry 模塊的入口文件。依賴項數組中所有的文件會按順序打包，每個文件進行依賴的查找，直到所有模塊都被打成包；

output：模塊的輸出文件，其中有如下參數：

filename: 打包後的文件名

path: 打包文件存放的絕對路徑。

publicPath: 網站運行時的訪問路徑。

relolve.extensions: 自動擴展文件的後綴名，比如我們在require模塊的時候，可以不用寫後綴名的。

relolve.alias: 模塊別名定義，方便後續直接引用別名，無須多寫長長的地址

plugins 附加插件;
```

##有關module中的loader

```
module: {
    loaders: [
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [
          path.resolve(__dirname, 'index.js'),
          path.resolve(__dirname, 'config.js'),
          path.resolve(__dirname, 'lib'),
          path.resolve(__dirname, 'app'),
          path.resolve(__dirname, 'src')
        ],
        exclude: [
          path.resolve(__dirname, 'test', 'test.build.js')
        ],
        cacheDirectory: true,
        query: {
          presets: ['es2015']
        }
      },
    ]
}
```
各項說明
```
test — a regular expression that tests what kind of files to run through this loader. As you can see, we have added a regex to test all files with an es6 extension.

exclude — which files the loader should exclude /ignore. We have added the node_modules folder.
loader — the name of the loader we are going to use (babel-loader).

query — You can pass options to the loader by writing them as a query string or by using the query property as we have done above.

cacheDirectory — Default false. When set, the given directory will be used to cache the results of the loader. Future webpack builds will attempt to read from the cache to avoid needing to run the potentially expensive Babel recompilation process on each run

presets — lets us use the react and es2015 presets that were installed earlier

include - defines the set of path or files where the imported files will be transformed by the loader.
```
