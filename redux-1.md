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


