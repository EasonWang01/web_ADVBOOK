#使用Babel,ESLint

# #Babel
https://babeljs.io/

一個解決舊版瀏覽器或node.js不相容新的js語法的方法，他可以把新語法compile    成相同功能的舊語法，讓舊瀏覽器也可以讀懂

例如:

```
items.map(item => item + 1);

// babel compile過後
items.map(function (item) {
  return item + 1;
});
```

稍後會講到的ESLint與webpack和現在講到的babel一樣都有一個配置文件，

類似一個菜單，寫好後交給程式，之後程式即可知道我們想要他其中的哪些功能

格式類似如下

```

{
  "presets": [],
  "plugins": []
}
```
上面的presets部分裡面會包含一些我們想讓babel有能力compile哪些版本的js新功能

例如:
```
  {
    "presets": [
      "es2015", //提供ES6功能
      "react",  // 可compile React的語法
      "stage-0" //提供ES7功能
    ],
    "plugins": []
  }
```
但記得寫在babel描述檔`babel.rc`文件內的`presets`要先安裝好他的npm套件才有效果

```

$ npm install --save-dev babel-preset-es2015
$ npm install --save-dev babel-preset-react
$ npm install --save-dev babel-preset-stage-0
```

#### Babel工具

1.Babel-cli
一個命令列工具

我們先安裝 `npm install -g babel-cli`

接著安裝 `npm i --save-dev babel-preset-es2015` 

新增一個`.babelrc`文件

```
{
   "presets": ["es2015"]
}
```
並在test1.js輸入

```
class Animal { 
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    console.log(this.name + ' makes a noise.');
  }
}

class Dog extends Animal {
  speak() {
    console.log(this.name + ' barks.');
  }
}
```

在命令列輸入 `babel test1.js -o test2.js
`
即會產生一個轉碼後的檔案test2.js

Babel還包含其他相關工具，這裡只稍微簡略介紹大概概念，其他可參考其官網，另外開發程式時babel可以與後面介紹的webpack搭配，將在稍後介紹

https://babeljs.io/docs/setup/

#### #ESLint

功能：定義一套程式碼規則，例如每段程式開頭幾個空格，或是逗點後要空一格等等，這些定義規則一樣會寫成一份文件，之後可搭配編輯器的plugin使用

我們會先寫一份`.eslintrc`檔案，上面定義了你寫要開啟它些規則，讓他偵測你的程式碼是否符合這些規則

# ESLint

##用處:用來檢查code的語法
1.安裝

```
npm install -g eslint  
```
2.配置

```
於根目錄新增   .eslintrc.json
```
```
{
  "globals": {
    // Put things like jQuery, etc
    "jQuery": true,
    "$": true
  },
  "env": {
    // I write for browser
    "browser": true,
    // in CommonJS
    "node": true
  },
  // To give you an idea how to override rule options:
  "rules": {
    // Tons of rules you can use, for example...
    "quotes": [1, "double"]
  }
}
```
看到上面的rules的數字，對照下表，以上面為例，意思是對所有出現quotes(引號)的地方要使用double(兩個引號)，強制性為1(只產生警告訊息)
```
0 - Disable the rule
1 - Warn about the rule
2 - Throw error about the rule
```
##其他rules可參考官網
http://eslint.org/docs/rules/

#如何執行?
```
eslint 檔案名稱.js
```

#也可使用別人寫好的lint


https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb

#安裝編輯器的插件
在插件搜尋eslint即可

###使用sublime+ESLint
進入sublime的package manage(需先安裝)

輸入
```
SublimeLinter-eslint
```


參考至
http://jonathancreamer.com/setup-eslint-with-es6-in-sublime-text/

