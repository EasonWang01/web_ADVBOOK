基礎

打開如下網址
http://codepen.io/gaearon/pen/ZpvBNJ

試著把script的內容改為
```
var HelloMessage = React.createClass({
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});

ReactDOM.render(<HelloMessage name="hihi" />, document.getElementById('root'));
```

也可以試著把script的內容改為（使用ES6）

```
class HelloMessage extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}

ReactDOM.render(<HelloMessage name="apple"/>, document.getElementById('root'));

```


基本React 元件構造
```
class baseComponent extends React.Component{

  constructor(props){
    super(props);
  }

}
```

####2.HTML與JSX

以前我們將HTML和JS分開寫，但React將頁面的畫面和邏輯全寫在JS內，而這個JS名稱為JSX，下面為JSX和HTML的簡單轉換對照圖

HTML
```html
<div class="banana" style="border: 1px solid red">
  <label for="name">Enter your name: </label>
  <input type="text" id="name" />
</div>
<p>Enter your HTML here</p>
```

```javascript
class baseComponent extends Component{

  render() {
    return (
    <div>
      <div className="banana" style={{border: '1px solid red'}}>
        <label htmlFor="name">Enter your name: </label>
        <input type="text" id="name" />
      </div>
      <p>Enter your HTML here</p>
    </div>
  );
 }
});
```

1. style改為物件的寫法
2. class改為className
3. React的component名字字母須大寫(與一般HTML tag區別)
4. function直接寫 `functionName() { }`  即可

####3.Component內傳遞data方式

一個component指的是一個class，下面為最基本的ES6 React component構造
```javascript
class TodoInput extends Component{
  render(){
    return(
      <div>
      </div>
    )
  }
}
```
而傳遞方式在我們使用Redux等框架前，主要使用state和prop傳遞

1. Props 主要為父元件傳遞下來的屬性
2. state為在一個component中容易被改變的data

簡單來說，props通常主要是用來父子間傳遞data用，而state是來記錄data目前的數值的，例如像server request後將data先存到state再改變畫面

####props範例

一個名為 Comment 的component
```
class Comment extends React.Component{
  render() {
    return (
      <div className="comment">
        <h1 className="commentAuthor">
          {this.props.author}
        </h1>
          {this.props.children}
      </div>
    );
  }
};
```
在父component呼叫
```
class CommentList extends React.Component{
  render() {
    return (
    <div >
      <Comment author="yicheng">Just do it</Comment>
    </div>
    );
  }
};
```

試著把剛才codepen改為如下

```
class Comment extends React.Component{
  render() {
    return (
      <div className="comment">
      <h1 className="commentAuthor">
      {this.props.author}
      </h1>
      {this.props.children}
      </div>
    );
  }
};

class CommentList extends React.Component{
  render() {
    return (
    <div >
      <Comment author="hihi">Just do it</Comment>
    </div>
    );
  }
};

ReactDOM.render(<CommentList />, document.getElementById('root'));
```

而最後會產生如下
```
<div className="comment">
  <h2 className="commentAuthor">
  yicheng
  </h2>
  Just do it
</div>
```
####State範例

接著把codepen改為

```
class CommentBox extends React.Component {
  constructor() {
    super();
    this.state = {
      data: 'hello'
    };
  }
  render() {
    return (
      <div className="commentBox">
        <h1>{this.state.data}</h1>
      </div>
   );
  }
};

ReactDOM.render(
  <CommentBox />,
  document.getElementById('root')
);
```
將產生如下
```
<h1>hello</h1>
```

你可能會看過`getInitialState`但在ES6可直接寫為如上即可

而設定state可用`setState()`

把codepen改為如下，點擊按鈕改變state

```
class CommentBox extends React.Component {
  constructor() {
    super();
    this.state = {
      data: 'hello'
    };
  }
  clickBtn = () => {
    this.setState({data: 'okok'})
  };
  render() {
    return (
      <div className="commentBox">
        <h1>{this.state.data}</h1>
        <button onClick={() => this.clickBtn()} />
      </div>
   );
  }
};

ReactDOM.render(
  <CommentBox />,
  document.getElementById('root')
);
```
