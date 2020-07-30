# 結合Node.js與React.js搭建論壇網站2

> 稍後記得把範例的mLab URL與帳號密碼改為你自己的

這部分我們主要會結合Socket.io，並且當有人發表文章後各個使用者的畫面都會即時顯示

先下載此單元的壓縮檔，並且使用`npm install`

1.文章Schema

```text
exports.Post = mongoose.model('articles', new mongoose.Schema({
  posterAccount: String,
  posterName: String,
  title: String,
  content: String,
  PostDate: String
}));
```

2.發表文章API

```text
app.post('/postArticle',function(req,res) {
    if(typeof req.session.user === 'string') {
        let post = new Post({
          posterAccount: req.body.account,
          posterName: req.body.name,
          title: req.body.title,
          content: req.body.content,
          PostDate: new Date()
        });
        post.save()
        .then(() => {
            res.end('發表文章成功');
        })
        .catch(err => {
            res.end('發表文章錯誤');
        });
    }
})
```

3./util/ArticleModal.js

```text
    axios.post('/postArticle', {
        user: this.props.user.name,
        account: this.props.user.account,
        content: this.state.content,
        title: this.state.title,
      })
    .then((response) => {
      context.setState({ dialog:true })
      context.setState({ dialogText:response.data })
      socket.emit('postArticle',{data:'test'});
    })
    .catch((e) => {
      console.log(e)
    })
```

4.server.js

```text
io.on('connection', function(socket){
  console.log('a user connected');
    socket.on('postArticle', function(){
        post({
            host: 'localhost',
            port: '3001',
            path: '/getArticle'
        },'hi')
        .then(function(data){
            socket.broadcast.emit('updateArticle',JSON.parse(data));
            socket.emit('updateArticle',JSON.parse(data));
        });
  });
});
```

