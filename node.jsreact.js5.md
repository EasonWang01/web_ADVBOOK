# 結合Node.js與React.js搭建論壇網站5

> 稍後記得把範例的mLab URL與帳號密碼改為你自己的

一樣先下載此單元的課程壓縮檔，然後解壓縮，之後使用`terminal`進入後輸入`npm install`

## 1.FB OAuth

我們先到他的API網站 [https://developers.facebook.com/docs/facebook-login/web](https://developers.facebook.com/docs/facebook-login/web)

點選右上方選單之後新增應用程式，新增完後再次點擊選單並進入該應用程式控制主頁

1.然後進入設定把`應用程式網域`與`網站網址`更改 ![](.gitbook/assets/螢幕快照%202017-02-11%20下午11.19.44.png)

2.然後把`應用程式編號`複製並貼到App.js的SDK中

![](.gitbook/assets/螢幕快照%202017-02-11%20下午11.16.40.png)

3.以及貼到Header.js 的`FB.init`的`appId`

![](.gitbook/assets/螢幕快照%202017-02-12%20上午11.54.26.png)

4.App.js:20 \(加入FB SDK\)

5.Header.js:46 \(初始化與發出登入request\)

6.api.js:120 \(寫上FBLogin API\)

## 2.搜尋文章

我們在Main.js放入搜尋框

```javascript
<TextField
  style={{position: 'absolute', top: '50px', left: '15%'}}
  hintText="搜尋文章..."
  underlineStyle={{borderColor: '#EC407A'}}
  onChange={(e) => this.searchArticle(e)}
/>
```

觸發

```javascript
  searchArticle(e) {
    if (e.target.value.length < 1){ //如果輸入框空白
      axios.get('/getArticle')
      .then((res) => {
        console.log(res.data);
        this.setState({FilterArticles: res.data})
      })    
      return 
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

```javascript
app.get('/articles/title/:title', (req,res) => {
  let re = new RegExp(req.params.title, "g");
    Post.find({title: re })
    .then(data => {
       res.end(JSON.stringify(data))
    })
})
```

## 3.線上聊天室使用者欄位

Chatroom.js:121

```javascript
    socket.on('chatRoomUsers', (res) => {
      if(this.state.users !== res.user) {
        this.setState({ users: res.user });
      };
    })
```

Chatroom.js:161

```javascript
       {      
          Object.keys(users).map(function(objectKey, index) {
              let name = users[objectKey].name;
              let avatar = users[objectKey].avatar;
              return (
                <IconButton key={index} tooltip={name}>
                  <Avatar src={avatar} />
                </IconButton>
              )
          })
        }
```

