

## 第二階段

開始新增其他react元件

在components下，新增一個檔案
`TextDisplay.js`

```
import React, {Component} from 'react'

class TextDisplay extend Component{

}

export default TextDisplay
```

上面是引用react後建造一個空的class後將他輸出

接著我們要在class內寫入東西

```
import React, {Component} from 'react'
//JSX要看到import了React 才可以編譯
class TextDisplay extends Component{

render() {
return (
<div>
<div> THis is text display</div>
<div> if we have two div we need to wrap it.</div>
</div>

)};

}

export default TextDisplay
```

使著將最外層的div刪掉，會出現錯誤，因為一個元件要有東西包住最外層。

之後讓原來的App.js引用他

```
import React, { Component } from 'react'

class App extends Component {

render() {
return (
<div>
<div>This is definitely a React app now!</div>
<TextDisplay/>
</div>
)}

}
export default App
```

使用state

TextDisplay.js

```
import React, { Component } from 'react'


class TextInput extends Component {

constructor() {
super()
this.state = {
inputText: ' sdxt'
}
}



render() {
return (
<div>
<input
type="text"
placeholder="This is going to be text"
value={this.state.inputText}

/>

</div>
)
}

}

export default TextInput
```

#### !!記得重新整理網頁，才會作用\(因為這裡是constructor\)

# 為元件加入方法

```
import React, { Component } from 'react'


class TextInput extends Component {

constructor() {
super()
this.state = {
inputText: ' sdsxt'
}
}

handleChange(){
console.log("ch")
}

render() {
return (
<div>
<input
type="text"
placeholder="This is going to be text"
value={this.state.inputText}
onChange={this.handleChange}
/>

</div>
)
}

}

export default TextInput
```

## 在class中的方法如果有this的話他會不知道this是什麼，所以要在class 的constructor中把該方法綁進來

1.

```
import React, { Component } from 'react'


class TextInput extends Component {

constructor() {
super()
this.state = {
inputText: ' sdsxt'
}
this.handleChange = this.handleChange.bind(this);
}

handleChange(){
this.setState({inputText:12});
}

render() {
return (
<div>
<input
type="text"
placeholder="This is going to be text"
value={this.state.inputText}
onChange={this.handleChange}
/>

</div>
)
}

}

export default TextInput
```

但後來發現如果想傳入參數還是要在html tag中寫bind才會傳入

2.所以另一種寫法，是直接在DOM 的onchange中綁，但官方推薦綁在constructor

```
onChange={this.handleChange.bind(this)}
```

接著在render上面寫

```
handleChange(){
...
}
```

3.第三種寫法\(ES6的箭頭函數，最方便，因為會直接幫你綁定\)

```
send = () => {
console.log(this.inputFiled.value)
let text = this.inputFiled.value;
this.props.addTodo1(text);
}


<button onClick={()=>this.handleSubmit()}>Submit</button>
```

如要傳入事件，記得兩邊\(\)都要傳入

```
<form onSubmit={(e)=>this.handleSubmit(e)}>
```

好處是不用再用bind

參考:[http://egorsmirnov.me/2015/08/16/react-and-es6-part3.html](http://egorsmirnov.me/2015/08/16/react-and-es6-part3.html)

#### !每次改動constructor記得都要重新整理，就算有用Hot reload

完整範例：

`注意其中的onclick 與 從子元件傳上來的 onclick`

container

```
import React, { Component } from 'react'
import {connect} from 'react-redux'
import actions from '../redux/actions/todoActions.js'
import { bindActionCreators } from 'redux'
import List from '../components/List.js'

class TodoList extends Component {
send = () => {
let text = this.inputFiled.value;
this.props.addTodo1(text);
}
itemClick = (e,id) => {
console.log(e)
console.log(id)
}
render() {
return (
<div>
<input ref={(c) => this.inputFiled = c} />
<button onClick={()=>this.send()}></button>
<List list={this.props} itemClick={(e,id)=>this.itemClick(e,id)}>
</List>
</div>
)
}

}
function mapStateToProp(state){
return state
}

function mapDispatchToProps(dispatch) {
return bindActionCreators({
addTodo1:actions.addTodo
},dispatch);
}

export default connect(mapStateToProp,mapDispatchToProps)(TodoList)
```

component

```
import React from 'react'
const List = (props) => {
let todos = Array.from(props.list.todos);
return (
<div>
{todos.map( i =>
<p key={i.id} onClick={(e)=>props.itemClick(e,i.id)}>{i.text}</p>
)}
</div>
)
}

export default List;
```

# 2.在class內所有的this都是指到那個class

所以要取得onchange時input內的value必須用e.target
，因為這裡不是DOM

```
import React, { Component } from 'react'


class TextInput extends Component {

constructor() {
super()
this.state = {
inputText: ' sdst'
}
this.handleChange = this.handleChange.bind(this);
}

handleChange(e){
console.log(e.target.value);
console.log(this);
//this.setState({inputText:12});
}

render() {
return (
<div>

<input onChange={this.handleChange} />
</div>
)
}

}

export default TextInput
```