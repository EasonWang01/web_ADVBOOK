3.接著我們幫他加入發出action後所要做的事

我們會使用array的filter方法，過濾出state.todos中completed為false的方法

(給點擊active按鈕用)反之為給completed按鈕用
```
var filtered = (this.props.todos).filter(function(state){
return state.completed==false

});
console.log(filtered)
```
(filter()讓array中每個元素接受一個function的運算且return一個值，為true或false)
(如果為true則繼續保留在array)

但

在這之前

我們先幫reducer加上一個狀態
```
let getId = 1 ;

export default function reducer(state,action){
switch(action.type){
case 'ADD_TODO':
return(	Object.assign({},state,{
todos:[{
text:action.text,
completed:false,
id:getId++

},...state.todos]
})
)
case 'TOGGLE_TODO':

return Object.assign({},state,{todos:state.todos.map(function(state){
if(state.id!==action.id){
return state
};

return {...state,completed:!state.completed}
}) }
)

case 'SET_VISBILITY_FILTER':

return Object.assign({},state,{visbility:action.filter})


default:
return state;

}

}
```
接著將App.js改為
```
import React, { Component } from 'react'
import TodoInput from './TodoInput.js'
import TodoList from './TodoList.js'
import {connect} from 'react-redux'




class App extends Component {

render() {
return (
<div>
<h1>Todo list</h1>
<TodoInput />
<TodoList todos={this.props}/>
</div>
)
}

}
function mapStateToProp(state){

return state
}


export default connect(mapStateToProp)(App)

```
(因為我們現在state裡不只一個state，所以先傳入整包，於子代再做取出)

最後

(把原本傳入最後return要map的物件先做過濾)


TodoList.js

```
import React, { Component } from 'react'
import action from '../redux/actions.js'
import store from '../redux/store'
import FilterLink from './FilterLink.js'

class TodoList extends Component {

constructor(props, context) {
super(props, context)
}





liClick(a){
store.dispatch(action.toggleTodo(a.id));

}




render() {
var filtered = function(){
switch(this.props.todos.visbility){

case "SHOW_ALL":
return this.props.todos.todos;

case "SHOW_ACTIVE":
return (this.props.todos.todos).filter(function(state){
return state.completed==false

});

case "SHOW_COMPLETED":
return (this.props.todos.todos).filter(function(state){
return state.completed==true

});
default:
return this.props.todos.todos;
}
}.bind(this)
return (
<div>
<ul>
{
filtered().map((todo)=>{
return <li
key={todo.id}
onClick={()=>this.liClick(todo)}
style= {{textDecoration:todo.completed?'line-through':'none'}}
>
{todo.text}

</li>
})
}

</ul>
<p>
{"Show: "}

<FilterLink filter="SHOW_ALL">
All
</FilterLink>
{" , "}
<FilterLink filter="SHOW_ACTIVE">
Active
</FilterLink>
{" , "}
<FilterLink filter="SHOW_COMPLETED">
Completed
</FilterLink>

</p>
</div>
)
}

}

export default TodoList

```
使用bind是因為我們在function內想取得class的執行環境，所以把他綁到this

3.接著

讓點擊後的link變黑色，無法重複點擊

所以先幫FilterLink加上一個props
```
import React, { Component } from 'react'
import action from '../redux/actions.js'
import store from '../redux/store'
import FilterLink from './FilterLink.js'

class TodoList extends Component {

constructor(props, context) {
super(props, context)
}





liClick(a){
store.dispatch(action.toggleTodo(a.id));

}




render() {
var filtered = function(){
switch(this.props.todos.visbility){

case "SHOW_ALL":
return this.props.todos.todos;

case "SHOW_ACTIVE":
return (this.props.todos.todos).filter(function(state){
return state.completed==false

});

case "SHOW_COMPLETED":
return (this.props.todos.todos).filter(function(state){
return state.completed==true

});
default:
return this.props.todos.todos;
}
}.bind(this)
/*
var filtered = (this.props.todos).filter(function(state){
return state.completed==false

});*/
//console.log(filtered)

return (
<div>
<ul>
{
filtered(
).map((todo)=>{
return <li
key={todo.id}
onClick={()=>this.liClick(todo)}
style= {{textDecoration:todo.completed?'line-through':'none'}}
>
{todo.text}

</li>
})
}

</ul>
<p>
{"Show: "}

<FilterLink filter="SHOW_ALL"currentFilter={this.props.todos.visbility}>
All
</FilterLink>
{" , "}
<FilterLink filter="SHOW_ACTIVE"currentFilter={this.props.todos.visbility}>
Active
</FilterLink>
{" , "}
<FilterLink filter="SHOW_COMPLETED"currentFilter={this.props.todos.visbility}>
Completed
</FilterLink>

</p>
</div>
)
}

}

export default TodoList

```
之後點擊link後去偵測，這個link的filter props和當前store的filter屬性如符合的話，則只回傳普通的文字
```
import React, { Component } from 'react'
import action from '../redux/actions.js'
import store from '../redux/store'
class FliterLink extends Component {

render(){
if(this.props.currentFilter==this.props.filter){
return <span> {this.props.children}</span>
}

return	<a href='#'
onClick={e=>{
e.preventDefault();
//console.log(this.props.filter)
store.dispatch(action.FilterTodo(this.props.filter))
}}
>
{this.props.children}
</a>
}



}

export default FliterLink
```
#使用combined reduecer

在這之前，我們先改變我們reducer的寫法

```javascript

let getId = 1 ;

function todos(state,action){
switch(action.type){
case 'ADD_TODO':
return [{
text:action.text,
completed:false,
id:getId++

},...state.todos]
case 'TOGGLE_TODO':

return state.todos.map(function(state){
if(state.id!==action.id){
return state
};

return {...state,completed:!state.completed}
})

default:
return state.todos;

}

}



function visbility(state,action){
switch(action.type){
case 'SET_VISBILITY_FILTER':

return action.filter

default:
return state.visbility;
}
}





function reducer (state,action){
return	Object.assign({},state,{
visbility:visbility(state,action),
todos:todos(state,action)

});

}


export default reducer
```
(上面，我們將reducer寫成兩個function)

combined reducer即是這個概念，所以我們把reducer.js 改為如下
```javascript
import { combineReducers } from 'redux'
let getId = 1 ;

function todos(state=[],action){
switch(action.type){
case 'ADD_TODO':
return [{
text:action.text,
completed:false,
id:getId++

},...state]
case 'TOGGLE_TODO':

return state.map(function(state){
if(state.id!==action.id){
return state
};

return {...state,completed:!state.completed}
})

default:
return state;

}

}



function visbility(state="SHOW_ALL",action){
switch(action.type){
case 'SET_VISBILITY_FILTER':

return action.filter

default:
return state;
}
}



const rootReducer = combineReducers({
visbility: visbility,
todos:todos
})


export default rootReducer
```
兩個重點
```
1. combineReducers 沒有丟預設state進去所以，我們要用ES6的寫法幫function寫上預設參數(Line 4 and 35)
2. function內一開始傳進去的的不再是整個state而是取key後的state，所以裡面也須更改(Line 13. 27.17.43)
```
最後，在ES6在物件的key跟value名稱相同時可省略

所以寫成
```
const rootReducer = combineReducers({
visbility,
todos,
})
```
>在多人合作中，我們會把reducer每個function放在不同檔案，最後再import到rootReducer來減少多人一起開發功能時的conflict

#中間件BindActionCreator

將store 的dispatch 包住action 成為function 可直接使用 不用再dispatch(someaction)

App.js加上
```
import { bindActionCreators } from 'redux';
import actions from '../redux/actions.js'

```
```
function mapDispatchToProps(dispatch) {
return {
actions: bindActionCreators(actions, dispatch)
}
}

export default connect(mapStateToProps, mapDispatchToProps)(App)
```
即可發現點選React devtool中App的props的dispatch變為actions物件

1.

原本父元件使用`this.props.dispatch`將dispatch方法傳下去，現在改為用`this.props.actions`

2.

而VIEW發送ACTION的方式從
```
handleSubmit(event) {
event.preventDefault()
this.props.dispatch(actions.addTodo(this.state.inputText))
}
```
改為
```
handleSubmit(event) {
event.preventDefault()
this.props.addTodo(this.state.inputText)
}
```

但一般我們不會用上面這種寫法

#mapDispatchToProp
建議如下寫法


https://egghead.io/lessons/javascript-redux-using-mapdispatchtoprops-shorthand-notation

```
//直接把action轉為function使用，因react-redux 幫你把dispatch包在裡面了

export default connect(mapStateToProp,{
userInfoAction:actions.userInfo,
})(Login)
```

#操作非同步動作(Async)
例如:
>我們今天有一個按鈕，點擊後發出action去跟server要資料，回傳資料後再觸發一個action去更新state

###使用Redux-thunk

`npm install redux-thunk`

將store.js改為
```
import {applyMiddleware,compose,createStore} from "redux"
import reducer from './reducer'
import logger from 'redux-logger'
import thunk from 'redux-thunk'

let initialState = {
visbility:'SHOW_ALL',
todos:[{
id:0,
completed: false,
text:'initial for demo'

}]
}


let finalCreateStore = compose(
applyMiddleware(thunk,logger())
)(createStore)

function configureStore(initialState){
return finalCreateStore(reducer,initialState)

}

let store = configureStore(initialState)

export default store
```
之後我們action.js裡面寫的方法不只可以return物件，還可以return function

這個function就是用來寫非同步API，最後拿回資料再寫return，之後即會送到reducer處理

例如:
```
FilterTodo:(filter)=>{
return({
type:'SET_VISBILITY_FILTER',
filter:filter

})
},
///上面是一般的action，下面是thunk的action

con:()=>{
return (dispatch,getState)=>{
console.log(getState())
}
}
```
PS:

在action.js內可以直接調用store的所有方法，因為我們action是由store發出的
ex:`store.dispatch(action.toggleTodo(a.id));```

#React-router-redux

>用途:將Redux結合react-router


安裝
`npm install --save react-router-redux`


1.在使用combind reducer的地方加上

```
import {routerReducer} from 'react-router-redux'


const rootReducer = combineReducers({
visbility,
todos,
routing: routerReducer,
})

```

2.將client.js加上
```
import { Router, Route, browserHistory } from 'react-router'
import { syncHistoryWithStore} from 'react-router-redux'

const history = syncHistoryWithStore(browserHistory, store)

```
完整版
```
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import store from '../redux/store'
import {Provider} from 'react-redux'
import { Router, Route, browserHistory } from 'react-router'
import { syncHistoryWithStore} from 'react-router-redux'
import TodoList from '../components/TodoList.js'
import TodoInput from '../components/TodoInput.js'

const history = syncHistoryWithStore(browserHistory, store)

render(
<Provider store={store} >
<Router history={history}>
<Route path="/" foo="bar" component={App}>
<Route path="/as" component={TodoList}/>
<Route path="/ass" component={TodoInput}/>
</Route>
</Router>
</Provider>,
document.getElementById('app')
)

```

最後將App.js改為
```
import React, { Component } from 'react'
import TodoInput from './TodoInput.js'
import TodoList from './TodoList.js'
import {connect} from 'react-redux'

class App extends Component {

render() {
return (
<div>
<h1>Todo list</h1>
{React.cloneElement(this.props.children, { todos:this.props })}
</div>
)
}

}
function mapStateToProp(state){

return state
}


export default connect(mapStateToProp)(App)

```
使用`React.cloneElement`原因為，原本react-router要顯示子帶router必須用{this.props.children}但這樣原本的prop沒辦法往下傳

所以使用`React.cloneElement`即可在子代傳入props

###記得加條件限制

但是，如果url為http://localhost:3000/

這時
`{React.cloneElement(this.props.children, { todos:this.props })}`

中的`this.props.children`為null，所以會報錯

須改為

```
import React, { Component } from 'react'
import TodoInput from './TodoInput.js'
import TodoList from './TodoList.js'
import {connect} from 'react-redux'

class App extends Component {


checkIfRouteHasChild(props){
if(props.children!=null){
return React.cloneElement(props.children, { todos:props})
}else{
return
}};


render() {


return (
<div>
<h1>Todo list</h1>
{(()=>this.checkIfRouteHasChild(this.props))()}
</div>
)
}

}
function mapStateToProp(state){

return state
}


export default connect(mapStateToProp)(App)

```

#Redux Devtools

1.到chrome extension 下載Redux Devtools

2.在程式的store.js中改為如下
```
let finalCreateStore = compose(
applyMiddleware(thunk,logger()),window.devToolsExtension ? window.devToolsExtension() : f => f
)(createStore)

function configureStore(initialState){
return finalCreateStore(reducer,initialState)

}

let store = configureStore(initialState)

export default store

```

3.開啟網頁即可看到chrome extension的redux devtools亮起，即可點選開啟


#stateless function components
#(可將class改為const)
#官方推薦使用
https://facebook.github.io/react/docs/reusable-components.html#stateless-functions

https://medium.com/@joshblack/stateless-components-in-react-0-14-f9798f8b992d#.nyhbdhy1j

因為Redux 的state統一存在store中，所以符合stateless function components之條件，
可改用const來寫component

```
const ListOfNumbers = props => (
<ol className={props.className}>
{
props.numbers.map(number => (
<li>{number}</li>)
)
}
</ol>
);

ListOfNumbers.propTypes = {
className: React.PropTypes.string.isRequired,
numbers: React.PropTypes.arrayOf(React.PropTypes.number)
};
```
####注意，使用const宣告的component裡面不可有state以及ref


-----
#從store 給到元件

1.
```
const mapStateToProp = (state) => ({
userInfo: state.userInfo
})
```

2.
```
<TextField
hintText=""
type="date"
id="date1"
value={ this.state.date }
onChange={ (e) => this.changeText(e,'date') }
floatingLabelStyle={{color: 'gray'}}
floatingLabelText=""
/>
```
3.
```
componentWillReceiveProps(nextProps) {//重新整理使用，因componentWillMount時的this.props還沒抓到
if (nextProps.userInfo !== 'undefined'){
this.setState({ date: nextProps.userInfo.birthday})
}
}
componentWillMount() {//切換元件時使用，因componentWillReceiveProps不會再切換元件時觸發，但每次切換元件state會重置
this.setState({ date: this.props.userInfo.birthday})
}
```