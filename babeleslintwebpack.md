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

類似一個菜單，寫好後交給程式，之後程式即可知道我們想要他的哪些功能，哪些是不需要的