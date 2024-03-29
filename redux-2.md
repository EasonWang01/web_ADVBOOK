# Redux 基本概念2

## 使用React連結Redux

到19章先輸入`npm install`安裝package.json寫的模組

這章的幾個資料夾是延續我們上一張React的結構

之後安裝使用redux所需要的模組

```
npm install --save redux react-redux react-router-redux redux-logger
```

之後我們執行，然後看著程式碼進行講解

#### client.js

```
1.使用Provider包住router，之後元件使用connect即可獲得Redux store

2.syncHistoryWithStore讓瀏覽器的location同步到store中
```

![](<.gitbook/assets/螢幕快照 2017-02-11 上午9.49.41.png>)

![](<.gitbook/assets/螢幕快照 2017-02-11 上午9.49.48.png>)

#### 元件中

```
1.使用connect把Redux的store與元件結合

export default connect((state) => state ,{ FilterTodo })(FliterLink)

//第一個參數，等同為把整個store的state訂閱了，如果store發生改變則此元件也會更新
//如果改為(state) => state.apple  則只會訂閱有關store中apple的部分，只有store 的apple更新了才會更新組件

//第二個參數是把action 轉為可以dispatch出去的function，之後即可使用this.props.FilterTodo即個發出action
//或是可寫this.props.dispatch(FilterTodo(this.props.filter));
//如果connect第二個參數沒寫，則dispatch會加入到this.props中即可使用上行所述去發出action
```

#### store

1\.

```
let finalCreateStore = compose(
    applyMiddleware(thunk,logger())
    )(createStore)
//用來加入一些middleware
```

2\.

```
let initialState = {
    visbility:'SHOW_ALL',
    todos:[{
        id:0,
        completed: false,
        text:'initial for demo'
    }]
}


function configureStore(initialState){
    return finalCreateStore(reducer,initialState)
}

//把初始的state與reducer做結合
```

3\.

```
let store = configureStore(initialState)

export default store

//最後輸出store
```

## 操作非同步動作(Async)

例如:

> 我們今天有一個按鈕，點擊後發出action去跟server要資料，回傳資料後再觸發一個action去更新state

### 使用Redux-thunk

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
FilterTodo:(filter) => {
  return({
    type:'SET_VISBILITY_FILTER',
    filter:filter
  })
},
///上面是一般的action，下面是thunk的action

con:() => {
  return (dispatch, getState)=>{
    console.log(getState())
  }
}
```

我們可以把action加上如下

```
    con:() => {
        return (dispatch, getState) => {
            console.log('現在的Store是');
            console.log(getState());
        }
  }
```

之後TodoList.js加入

```
 <button onClick={() => this.props.dispatch(action.con())}  />
```

## Redux Devtools

1.到chrome extension 下載Redux Devtools

[https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=zh-TW](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=zh-TW)

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

\--試著發出一些action然後點選最下方第七個時鐘圖案，即可看到他可以重播我們做過的action

\--也可點選chart去做查看

\--可點上方的做過的action的JUMP按鈕，之後可到當時的情況，但JUMP過去後不可再做改變，也就是點選畫面會沒反應，必須再JUMP回最新的action才能繼續對畫面做點選(原因是設計者要讓我們可以觀看過去但不可更改，否則後續的流程都會亂掉)

![](<.gitbook/assets/螢幕快照 2017-02-11 上午9.56.31.png>)
