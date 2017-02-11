#第三部分

>稍後記得把範例的mLab URL與帳號密碼改為你自己的


一樣先下載此單元的課程壓縮檔，然後解壓縮，之後使用`terminal`進入後輸入`npm install`


1.Main.js
```
 <ArticleBlock articleClick={(e,id) => this.articleClick(e,id)} articles={this.props.articles} />

```
Main.js中的子元件ArticleBlock.js用來顯示主頁面所有文章

./components/utils/ArticleBlock.js
```javascript
const ArticleBlock = (props) => (
  <div style={style.articleContainer}>
    {
      props.articles.map((i) => (
        <div onClick={(e) => props.articleClick(e,i._id)} className="articleBlock"  style={style.article} key={i._id}>
          <div style={style.avatar}>
            <img height="50px" width="60px" src={i.avatar} />
          </div>
          <div style={style.title}>
            {i.title}
          </div>
          <div style={style.date}>{(i.PostDate)}</div>
        </div>
      ))
    }
  </div>
)
``` 
props.articles 來自Main.js中的mapStateToProp



2.Loading

參考:MyArticle.js

```
用來顯示讀取中狀態，之後可用條件判斷，如果是Ajax進行中就讓它顯示，讀取到資料再讓Loading消失
```

3.PersonalInfo.js


```
1.imgur API
2.signup api 在註冊時把信箱用md5加密，給avatar  http://www.gravatar.com/
```

4.signup 後使用nodemailer 傳送郵件

5.我的文章頁面可修改文章(MyArticle.js)




)