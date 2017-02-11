#第五部分

>稍後記得把範例的mLab URL與帳號密碼改為你自己的


一樣先下載此單元的課程壓縮檔，然後解壓縮，之後使用`terminal`進入後輸入`npm install`








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