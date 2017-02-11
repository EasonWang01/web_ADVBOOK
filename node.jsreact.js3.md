#第三部分

>稍後記得把範例的mLab URL與帳號密碼改為你自己的


一樣先下載此單元的課程壓縮檔，然後解壓縮，之後使用`terminal`進入後輸入`npm install`

ArticleBlock.js

將store中的中的

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

