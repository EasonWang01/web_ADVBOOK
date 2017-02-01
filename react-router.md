# 使用React router

`npm install react-router`

之後開啟client.js

改為下面，看是否仍正常啟動

```
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import { Router, Route, hashHistory } from 'react-router'

render(( 
    <Router history={hashHistory}>
     <Route path="/" component={App}/>
    </Router> 
  ),document.getElementById('app'))
```

再改為下面看看

```
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import Proptest from '../components/Proptest'
import { Router, Route, hashHistory } from 'react-router'

render(( 
    <Router history={hashHistory}>
    <Route path="/" component={App}/>
     <Route path="/about" component={Proptest}/>
    </Router> 
  ),document.getElementById('app'))
```

到路徑[http://localhost:3000/\#/about](http://localhost:3000/#/about)

即可看到，元件的切換

\(發現頁面切換元件很快速，我們以前要做到這樣必須用AJAX，或模板引擎內的動態compile\(一樣是AJAX加載\)，  
但React沒用到ajax，完全都在client端計算更改的virtual DOM後更新到DOM上\)

