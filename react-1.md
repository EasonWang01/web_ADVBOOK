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


基本React 元件構造介紹

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
<!-- compare this two-->
<div class="banana" style="border: 1px solid red">
<label for="name">Enter your name: </label>
<input type="text" id="name" />
</div>
<p>Enter your HTML here</p>
```
JSX
```
var NewComponent = React.createClass({
render: function() {
return (
<div>
{/* compare this two*/}
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
class Comment extends Component{
render() {
return (
<div className="comment">
<h2 className="commentAuthor">
{this.props.author}
</h2>
{this.props.children}
</div>
);
}
};
```
在父component呼叫
```
class CommentList extends Component{
render() {
return (
<div >
<Comment author="yicheng">Just do it</Comment>
</div>
);
}
});
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
```
class CommentBox extends Component{
state = {
data:hello
};
render() {
return (
<div className="commentBox">
<h1>{this.state.data}</h1>
</div>
);
}
});
```
將產生如下
```
<h1>hello</h1>
```

你可能會看過`getInitialState`但在ES6可直接寫為如上即可

而設定state可用`setState()`

下面介紹`componentDidMount`方法，用途為在元件載入到頁面後所要執行的函式，類似於jquery中的`$( document ).ready()`方法
```
componentDidMount: function() {
$.ajax({
url: this.props.url,
dataType: 'json',
cache: false,
success: function(data) {
this.setState({data: data});
}.bind(this),
error: function(xhr, status, err) {
console.error(this.props.url, status, err.toString());
}.bind(this)
});
},
```
上面範例，在元件載入後，發送了AJAX到props中寫的url並將取得的資料設定到元件的state中

講到這裡你可能會對上面寫的ES6 class不太理解，可參閱附錄1的ES6 class，再繼續

####React.js 於ES6 寫法

官網上的範例大多還沒引入ES6的class相關寫法，下面稍微介紹有關React.js 於ES6 的用法

```
class Fruit extends React.Component{
// 建構子
constructor(props){
super(props);
}
// 其他函式等
}
//定義另一類別，繼承Fruit
class Apple extends Fruit{
// 建構子
constructor(props){
super(props);
}
// 初始化state,替代原本getInitialState方法
state = {
eat:false
};
// 替代原propTypes 属性,注意前面有static,
//屬於靜態方法，propTypes用來定義資料型態
static propTypes = {
eat: React.PropTypes.bool.isRequired
}


// 這以用箭頭函數箭頭函數不會改變this的指向,否則函數內,this指的就不是當前對象了(於下面會講到)
// React.CreatClass方式React會自動綁定this,ES6寫法不會.
handleClick = (e)=>{
this.setState();//
};
componentDidMount() {
// 元件載入到頁面後執行 ,這裡要顯示所繼承父類的相同function,
// 否則一旦父類中有封裝,子類會把和父類相同的function覆蓋,不會執行父類的function.
// 但，父類如果本身沒有寫出componentDidMount,在子類寫出的話就會錯誤
//所以可以如下寫出

if (super.componentDidMount) {
super.componentDidMount();
}
// do something yourself...
}
}
//但通常我們React 的class都會繼承React.Component
//較少去直接繼承其他的component，所以上面所述較少用



//上面講到的propTypes也可寫在class外，改為如下
Apple.propTypes = {
title: PropTypes.string.isRequired
}
```

上面看完可能有點模糊

下面將會個別講解


####1.引用React
ES5
```
var React = require('react');
var ReactPropTypes = React.PropTypes;
```
ES6
```
import React, {Component, PropTypes} from 'react';
```
為了瞭解第一個React 在import時周圍不用加上{}，我們下面看一下其source code
```
var React = {
Component: ReactComponent,

PropTypes: ReactPropTypes,
}

module.exports = React;
```
因為React是使用module.exports所輸出的，類似於ES6的export default，類似整個輸出的概念，只有這兩個在引
用時不要在外面在上{}

接著看下面這個例子

ES5
```
var Header = React.createClass({
render: function() {
return (
<header>
<h1>This is the header section</h1>
</header>
);
}
});

module.exports = Header;
```
ES6
```
export default class Header extends Component {
render() {
return (
<header>
<h1>This is the header section</h1>
</header>
);
}
}
```
使用 'export default'時， import的人可以自己為他取名字, 但使用 'export' import的人必須將名字取為你在'export'時的名字
(可參考附錄2-module)

接著講PropTypes

ES5
```
var React = require('react');
var ReactPropTypes = React.PropTypes;

var Header = React.createClass({
propTypes: {
title: ReactPropTypes.string.isRequired
}
});
```
ES6
```
import React, {Component, PropTypes} from 'react';

export default class Header extends Component {
render() {
return (
<header>
<h1>This is the header section</h1>
</header>
);
}
}


Header.propTypes = {
title: PropTypes.string.isRequired
}
```

ES7
```
import React, {Component, PropTypes} from 'react';

export default class Header extends Component {



static propTypes = {
title: PropTypes.string.isRequired
}

render() {
return (
<header>
<h1>This is the header section</h1>
</header>
);
}
}
```
有關getInitialState
ES5
```
var Header = React.createClass({
getInitialState: function() {
return {
title: this.props.title
};
},
});
```
ES6

```
export default class Header extends Component {
constructor(props) {
super(props);
//constructor內寫到this前記得先寫好super()
this.state = {
title: props.title
};
}
}
```
ES7
```
export default class Header extends Component {
state = {
title: this.props.title
};

}
```
接著最後是有關在class呼叫其他function的方法

ES5
```
var Header = React.createClass({
handleClick: function(event) {
this.setState({liked: !this.state.liked}); //在這裡this會自動綁訂到這個class

}
});
```
但是在 ES6, React 開發團隊決定 不要讓this自動綁到class上.所以我們必須手動把他綁訂到 constructor()內

ES6
```
export default class Header extends Component {
constructor() {
super();
this.handleClick = this.handleClick.bind(this);
}
handleClick(event) {
this.setState({liked: !this.state.liked});
}
}
```

或是使用箭頭函式，可自動綁定this到該class
(箭頭函式可參照附錄3)
```
export default class Header extends Component {
handleClick = (event) => {
this.setState({liked: !this.state.liked});
}
}
```
而React開發團隊，建議上面這種寫法

