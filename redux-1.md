# Redux

#概念
views點擊=>action => reducer => store =>回傳state給views

1.state統一由store保存，任何更新state都要告知store

2.讓views得到store中state的方法
```
1.使用connect讓最上層元件取得Provider中的store，再用props傳下去

2.使用store.getState
```

新增test1.html

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

####Action

為一個函數返回一個物件，裡面至少會包含type

```
function addTodo(text) {
  return {
    type: ADD_TODO,
    text: 'hihi'
  }
}
```

####Reducer

Reducer為一個function裡面通常是寫入switch然後判斷剛才發出action的type，決定要做哪一種對應的switch case處理

```
function todoApp(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos,
          {
            text: action.text,
            completed: false
          }
        ]
      })
    default:
      return state
  }
}
```

上面的例子是，把原本的store中的state的todos陣列加入一筆新的資料

假設
```
var state = {
  todos : [{text:'hihi',completed: false},{text:'okok',completed: false}]
}

```

我們可用

```
console.log(...state.todos)
```

之後


```
var final = Object.assign({}, state, {
        todos: [
          ...state.todos,
          {
            text: 'yes',
            completed: false
          }
        ]
      })

console.log(final);

```