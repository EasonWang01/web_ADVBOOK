# Redux
source code
https://cdnjs.cloudflare.com/ajax/libs/redux/3.3.1/redux.js
#概念
views點擊=>action => reducer => store =>回傳state給views

1.state統一由store保存，任何更新state都要告知store

2.讓views得到store中state的方法
```
1.使用connect讓最上層元件取得Provider中的store，再用props傳下去

2.使用store.getState
```

新增test1.html

在terminal輸入`open test1.html`
#簡單範例
```
<!DOCTYPE html>
<html>
  <head>
    <title>Redux basic example</title>
    <script src="https://npmcdn.com/redux@latest/dist/redux.min.js"></script>
  </head>
  <body>
    <div>
      <p>
        Clicked: <span id="value">0</span> times
        <button id="increment">+</button>
        <button id="decrement">-</button>
        <button id="incrementIfOdd">Increment if odd</button>
        <button id="incrementAsync">Increment async</button>
      </p>
    </div>
  </body>
  <script>

    //初始化store
    var store = Redux.createStore(counter);

    //每當store更新時執行render function
    var valueEl = document.getElementById('value')
    function render() {
      valueEl.innerHTML = store.getState().toString()
    }
    render()
    store.subscribe(render);

    /*以下為Reducer  */
    function counter(state, action) {
      if (typeof state === 'undefined') {//初始狀態
        return 0
      }
        switch (action.type) {
        case 'INCREMENT':
          return state + 1
        case 'DECREMENT':
          return state - 1
        default:
          return state
      }
    }

    /*以下為action*/
    document.getElementById('increment')
        .addEventListener('click', function () {
          store.dispatch({ type: 'INCREMENT' })
        })
      document.getElementById('decrement')
        .addEventListener('click', function () {
          store.dispatch({ type: 'DECREMENT' })
        })
      document.getElementById('incrementIfOdd')
        .addEventListener('click', function () {
          if (store.getState() % 2 !== 0) {
            store.dispatch({ type: 'INCREMENT' })
          }
        })
      document.getElementById('incrementAsync')
        .addEventListener('click', function () {
          setTimeout(function () {
            store.dispatch({ type: 'INCREMENT' })
          }, 1000)
      })
  </script>
</html>
```

#使用React連結Redux

到19章先輸入`npm install`安裝package.json寫的模組


這章的幾個資料夾是延續我們上一張React的結構

之後安裝使用redux所需要的模組
```
npm install --save redux react-redux react-router-redux
```

client.js
```
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import Proptest from '../components/Proptest'
import TextDisplay from '../components/TextDisplay'
import Repo from '../components/Repo'
import { Router, Route, browserHistory } from 'react-router'
/*Redux 部分 */
import {Provider} from 'react-redux'
import {configureStore} from '../redux/store'

render(( /*在router外包一層Provider */
    <Provider store={store}>
        <Router history={browserHistory}>
            <Route path="/" component={App}>
                <Route path="/Proptest" component={Proptest}/>
                <Route path="/TextDisplay" component={TextDisplay}/>
                <Route path="/repo/:userName/:repoName" component={Repo}/>
            </Route>
        </Router> 
    </Provider>
  ),document.getElementById('app'))

```
Provider用來連結react即redux的store


5.

新增redux資料夾

裡面放入三個檔案

`store.js`  `action.js`  `reducer.js`

store.js

```
import {applyMiddleware,compose,createStore} from "redux"
import reducer from './reducer'
import logger from 'redux-logger'

let finalCreateStore = compose(
  applyMiddleware(logger())
)(createStore);

export default function configureStore(initialState = { todos:[] }) {
  return finalCreateStore(reducer,initialState)
}
```
這裡我們加入了中間件logger

而store的必要參數為reducer，我們用`import reducer from './reducer'`傳入

action.js
```
let actions ={
  addTodo:(text)=>{
    return ({
      type:'ADD_TODO',
      text:text })
    }
}


export default actions
```
reducer.js
```
let getId = 1 ;

export default function reducer(state, action){
  switch(action.type){
    case 'ADD_TODO':
      return(	Object.assign({}, state, {
        todos:[{
          text:action.text,
          completed:false,
          id:getId++
          }, ...state.todos]
        })
      )
    default:
      return state;
  }
}
```
最後建立component資料夾

裡面放入`App.js` `TodoInput.js`  `TodoList.js`

App.js
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
        <TodoInput dispatch={this.props.dispatch}/>
        <TodoList dispatch={this.props.dispatch} todos={this.props.todos}/>
      </div>
    )
  }

}
function  mapStateToProps(state){

	return state
}


export default connect(mapStateToProps)(App)

```
從chrome React dev tool可以看到如果使用connect

App元件可以接到從store來的state且轉成他的props

TodoInput.js
```
import React, { Component } from 'react'
import action from '../redux/actions.js'
class TodoInput extends Component {

  constructor(props, context) {
    super(props, context)
 
    this.handleSubmit = this.handleSubmit.bind(this);
  }


 
  handleSubmit(){
    event.preventDefault()
    //this.props.dispatch()
    console.log(this._input.value);
    this.props.dispatch(action.addTodo(this._input.value));
  }




  render() {
    return (
      <div>
        <input
          type="text"

          placeholder="Type in your tode"
     
          ref={(c) => this._input = c}
        />
        <button onClick={this.handleSubmit}>Submit</button>
      </div>
    )
  }

}

export default TodoInput

```
TodoList.js
```
import React, { Component } from 'react'

class TodoList extends Component {

  render() {
    return (
      <ul>
        {
          this.props.todos.map((todo)=>{
            return <li key={todo.id}> {todo.text}  </li>
          })
        }

      </ul>
    )
  }

}

export default TodoList

```
完整版:(於master branch)

https://github.com/EasonWang01/Redux-tutorial 

#接著幫他加上toggle
使點選後判斷完成了沒，來讓事項有一橫線

1.先在action.js
```
let  actions ={ 
	addTodo:(text)=>{
		return ({
	type:'ADD_TODO',
	text:text})
},
	toggleTodo:(id)=>{
		return({
	type:'TOGGLE_TODO',
	id:id,
	})

	}
}


export default actions
```
2.reducer.js

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
                return  state
                };

                return {...state,completed:!state.completed}
           
			}) }
      )


	

				

		default:
			return state;

	}

}
```
最後在TodoList.js
```
import React, { Component } from 'react'
import action from '../redux/actions.js'
import store from '../redux/store'
class TodoList extends Component {

   constructor(props, context) {
    super(props, context)
 
  }





  liClick(a){
   
      store.dispatch(action.toggleTodo(a.id));

  }




  render() {
    return (
      <ul>
        {
          this.props.todos.map((todo)=>{
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
    )
  }

}

export default TodoList

```
完整版在 branch toggle

#接著加入三個選項，分別顯示all,active,completed

1.新增FilterLink.js
```
import React, { Component } from 'react'
import action from '../redux/actions.js'
import store from '../redux/store'
class FliterLink extends Component {

	render(){


	return	<a  href='#'
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
2.將他加入TodoList.js

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
    return (
      <div>
      <ul>
        {
          this.props.todos.map((todo)=>{
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
          {"  ,  "}
          <FilterLink filter="SHOW_ACTIVE">
         Active
          </FilterLink>
          {"  ,  "}
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
更改action.js
```
let  actions ={ 
	addTodo:(text)=>{
		return ({
	type:'ADD_TODO',
	text:text})
},
	toggleTodo:(id)=>{
		return({
	type:'TOGGLE_TODO',
	id:id,
	})

	},
	FilterTodo:(filter)=>{
		return({
	type:'SET_VISBILITY_FILTER',
	filter:filter		

		})
	}
}


export default actions
```
以及reducer.js
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
                return  state
                };

                return {...state,completed:!state.completed}
           
			}) }
      )

      	case 'SET_VISBILITY_FILTER':

      	return state
	

				

		default:
			return state;

	}

}
```

(此時多了三個選項，且點擊會發出action)

