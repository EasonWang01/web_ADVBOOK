#第四部分

>稍後記得把範例的mLab URL與帳號密碼改為你自己的


一樣先下載此單元的課程壓縮檔，然後解壓縮，之後使用`terminal`進入後輸入`npm install`



####文章留言

LeaveMsgModal.js

####API 加上 token認證

使用JWT token
```javascript
const authToken = (req,res,next) => {
	const token = req.cookies.t;
	if (token) {
		jwt.verify(req.cookies.t, jwtSecret, (err, decoded) => {
			if(decoded){
				next();
			} else {
				res.end('token not correct');
			}
	  });
	} else {
		res.end('no Token');
	}
}
```

api.js:211
```javascript
app.put('/leavemsg',authToken,(req,res) => {
	Post.findOne({ _id: req.body.id })
	.then(data => {
		let newComments = data.comments;
		newComments.push({
			title : req.body.title,
			content : req.body.content,
			authorAccount : req.body.authorAccount,
			userAvatar: req.body.userAvatar,
			date: Date.now() + 1000 * 60 * 60 * 8
		})
	Post.update({ _id: req.body.id },{ $set : {
		comments: newComments,
		lastModify : Date.now() + 1000 * 60 * 60 * 8
		}})
		.then(data => {
			 res.end(JSON.stringify(data))
		})
	})
})
```
####聊天室

Chatroom.js


####Redis

1.一個使用者登入後，其他登入此帳號的裝置會被登出  login.js:59

2.記錄連線人數（顯示在terminal）  


