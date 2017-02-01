#使用React連結Redux

到19章先輸入`npm install`安裝package.json寫的模組


這章的幾個資料夾是延續我們上一張React的結構

之後安裝使用redux所需要的模組
```
npm install --save redux react-redux react-router-redux redux-logger
```

之後我們執行，然後看著程式碼進行講解

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