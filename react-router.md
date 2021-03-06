# react-router

## 使用React router

`npm install react-router --save`

之後開啟client.js

改為下面，看是否仍正常啟動

```text
import React from 'react'
import ReactDOM from 'react-dom'
import App from '../components/App'
import { Router, Route, hashHistory } from 'react-router'

ReactDOM.render((
  <Router history={hashHistory}>
    <Route path="/" component={App}/>
  </Router>
),document.getElementById('app'))
```

再改為下面看看

Client.js

```text
import React from 'react'
import ReactDOM from 'react-dom'
import App from '../components/App'
import Proptest from '../components/Proptest.js';
import TextDisplay from '../components/TextDisplay'
import { Router, Route, hashHistory } from 'react-router'

ReactDOM.render((
  <Router history={hashHistory}>
    <Route path="/" component={App}/>
    <Route path="/TextDisplay" component={TextDisplay}/>
    <Route path="/Proptest" component={Proptest}/>
  </Router>
),document.getElementById('app'))
```

Proptest.js

```text
import React, { Component } from 'react'

const Proptest = () => (
  <div>Proptest</div>
);

export default Proptest
```

TextDisplay.js

```text
import React, { Component } from 'react'

const TextDisplay = () => (
  <div>TextDisplay</div>
);

export default TextDisplay
```

到路徑[http://localhost:3000/\#/TextDisplay](http://localhost:3000/#/TextDisplay)

即可看到，元件的切換

\(發現頁面切換元件很快速，我們以前要做到這樣必須用AJAX，或模板引擎內的動態compile\(一樣是AJAX加載\)，  
而React沒用到ajax，是在client端計算更改的virtual DOM後更新到DOM上\)

### 2.Link

接著到App.js加上

```text
import React, { Component } from 'react'
import TextDisplay from './TextDisplay'
import { Link } from 'react-router'

class App extends Component {

  render() {
    return (
      <div>
        <ul role="nav">
          <li><Link to="/TextDisplay">TextDisplay</Link></li>
          <li><Link to="/Proptest">Proptest</Link></li>
        </ul>
        <TextDisplay/>
      </div>
    ) 
  }
}
export default App
```

即可看到點選li跳至不同元件，及更改了url

### 3.我們現在想讓App.js變成一個nav然後點選後App.js不動，在他的下面render不同的component，所以我們要把route改為巢狀

client.js

```text
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import Proptest from '../components/Proptest'
import TextDisplay from '../components/TextDisplay'
import { Router, Route, hashHistory } from 'react-router'

render(( 
    <Router history={hashHistory}>
        <Route path="/" component={App}>
             <Route path="/Proptest" component={Proptest}/>
             <Route path="/TextDisplay" component={TextDisplay}/>
        </Route>
    </Router> 
  ),document.getElementById('app'))
```

App.js

```text
import React, { Component } from 'react'
import TextDisplay from './TextDisplay'
import { Link } from 'react-router'

class App extends Component {

  render() {
    return (
    <div>
        <ul role="nav">
          <li><Link to="/TextDisplay">TextDisplay</Link></li>
          <li><Link to="/Proptest">Proptest</Link></li>
        </ul>
         {this.props.children}
    </div>
  )}

}
export default App
```

，因為今天兩個component在client.js下為App.js的子代，所以要用`{this.props.children}`去顯示 \(更改後記得重新整理\)

### 4.

幫link 加上active時的style

App.js

```text
import React, { Component } from 'react'
import TextDisplay from './TextDisplay'
import { Link } from 'react-router'

class App extends Component {

  render() {
    return (
    <div>
     <ul role="nav">
          <li><Link to="/TextDisplay" activeStyle={{ color: 'orange' }}>TextDisplay</Link></li>
          <li><Link to="/Proptest" activeStyle={{ color: 'red' }}>Proptest</Link></li>
        </ul>
         {this.props.children}

    </div>
  )}

}
export default App
```

> 或是像原本一樣寫`active`時的css也可以

### 5.像Express 使用參數url

新增一個元件`Repo.js`

```text
import React, { Component } from 'react'

const Repo = (props) => (
  <div>{props.params.repoName}</div>
);

export default Repo
```

更改Client.js

```text
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import Proptest from '../components/Proptest'
import TextDisplay from '../components/TextDisplay'
import Repo from '../components/Repo'
import { Router, Route, hashHistory } from 'react-router'

render(( 
    <Router history={hashHistory}>
        <Route path="/" component={App}>
             <Route path="/Proptest" component={Proptest}/>
             <Route path="/TextDisplay" component={TextDisplay}/>
             <Route path="/repo/:userName/:repoName" component={Repo}/>
        </Route>
    </Router> 
  ),document.getElementById('app'))
```

之後在url輸入[http://localhost:3000/\#/repo/this/is](http://localhost:3000/#/repo/this/is) 即可

> PS:巢狀route，當url是子代時，會把所有的父代component都render出

## 7.消除原本在url上的`#`

將hash history改為browser history

Client.js

```text
import { Router, Route, browserHistory  } from 'react-router'
```

```text
<Router history={browserHistory}>
```

參考: [https://github.com/reactjs/react-router/blob/master/docs/guides/Histories.md\#configuring-your-server](https://github.com/reactjs/react-router/blob/master/docs/guides/Histories.md#configuring-your-server)

參考至

[https://github.com/reactjs/react-router-tutorial/tree/master/lessons/02-rendering-a-route](https://github.com/reactjs/react-router-tutorial/tree/master/lessons/02-rendering-a-route)

