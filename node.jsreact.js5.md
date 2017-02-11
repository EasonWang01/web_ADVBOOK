#第五部分

>稍後記得把範例的mLab URL與帳號密碼改為你自己的


一樣先下載此單元的課程壓縮檔，然後解壓縮，之後使用`terminal`進入後輸入`npm install`



####FB OAuth

我們先到他的API網站
https://developers.facebook.com/docs/facebook-login/web

點選右上方選單之後新增應用程式，新增完後再次點擊選單並進入該應用程式控制主頁

然後進入設定把`應用程式網域`與`網站網址`更改
![](/assets/螢幕快照 2017-02-11 下午11.19.44.png)

然後把`應用程式編號`複製並貼到App.js的SDK中

![](/assets/螢幕快照 2017-02-11 下午11.16.40.png)


####搜尋文章

我們在Main.js放入搜尋框

```
<TextField
  style={{position: 'absolute', top: '50px', left: '15%'}}
  hintText="搜尋文章..."
  underlineStyle={{borderColor: '#EC407A'}}
  onBlur={(e) => this.searchArticle(e)}
/>
```

觸發
```
  searchArticle(e) {
    if (e.target.value.length < 1){ //如果輸入框空白
      axios.get('/getArticle')
      .then((res) => {
        console.log(res.data);
        this.setState({FilterArticles: res.data})
      })     
    };
    if(e.target.value.length > 20) {
      sweetAlert('不可超過20字');
      return
    }; 
    axios.get(`/articles/title/${e.target.value}`)
    .then((res) => {
      console.log(res.data);
      this.setState({FilterArticles: res.data})
    })
  }
```


然後加入搜尋title的API

api.js:81

```
app.get('/articles/title/:title', (req,res) => {
	console.log(req.params.title)
	Post.find({title: req.params.title})
	.then(data => {
  	 res.end(JSON.stringify(data))
	})
})
```