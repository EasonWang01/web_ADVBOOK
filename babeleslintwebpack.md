#使用Babel,ESLint與Webpack

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

Babel還包含其他相關工具，這裡簡略介紹大概概念，其他可參考其官網https://babeljs.io/docs/setup/

#### #ESLint
