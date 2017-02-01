# Prop

即為HTML tag中的屬性

1.新增一個元件為Proptest.js

```
import React, { Component } from 'react'

class Proptest extends Component {
  render(){
    return <div> {this.props.text}</div>
  }
}
export default Proptest
```

TextDisplay.js

```
import React, { Component } from 'react'
import Proptest from './Proptest.js';

class TextDisplay extends Component {

  constructor() {
    super()
  }

  render() {
    return (
      <div>
        <Proptest text="I am PropTest" />
      </div>
    )
  }
}

export default TextDisplay
```

即可看到Proptest的props顯示出來了

2.讓子代來啟動父代的function

Proptest.js

```
import React, { Component } from 'react'

class Proptest extends Component {
  render(){
    return <button onClick={this.props.trigger} />
  }
}
export default Proptest
```

TextDisplay.js

```
import React, { Component } from 'react'
import Proptest from './Proptest.js';

class TextDisplay extends Component {

  constructor() {
    super()
    this.state = {
      inputText: ' sdsxt'
    }
  }

  handleChange(e) {
    console.log('hihi')
  }

  render() {
    return (
      <div>
      <Proptest trigger={() => this.handleChange()} />
      </div>
    )
  }
}

export default TextDisplay
```


# 在元件內使用條件判斷

Proptest.js

```
import React, { Component } from 'react'

class Proptest extends Component {
  render(){
    return (
      <div>
        <p>點擊按鈕後我會消失</p>
      </div>  
    )
  }
}
export default Proptest
```
TextDisplay.js

```
import React, { Component } from 'react'
import Proptest from './Proptest.js';

class TextDisplay extends Component {

  constructor() {
    super()
    this.state = {
      show: true
    }
  }

  handleChange() {
    if(this.state.show) {
      this.setState({show: false});
    } else {
      this.setState({show: true});
    }
  }

  render() {
    return (
      <div>
        <button onClick={() => this.handleChange()} />
        { this.state.show ? <Proptest /> : '' }
      </div>
    )
  }
}

export default TextDisplay
```


#使用AJAX

1.先在server.js加上app.get的路徑

```
var express = require('express');
var path = require('path');
var config = require('../webpack.config.js');
var webpack = require('webpack');
var webpackDevMiddleware = require('webpack-dev-middleware');
var webpackHotMiddleware = require('webpack-hot-middleware');

var app = express();

var compiler = webpack(config);

app.use(webpackDevMiddleware(compiler, {noInfo: true, publicPath: config.output.publicPath}));
app.use(webpackHotMiddleware(compiler));

app.use(express.static('./dist'));

app.get('/hi',function(req,res){
  console.log(req);
  res.end("Hello Hello hi hi hi");
});

app.use('/', function (req, res) {
  res.sendFile(path.resolve('client/index.html'));
});


var port = 3000;

app.listen(port, function(error) {
  if (error) throw error;
  console.log("Express server listening on port", port);
});
```

2.TextDisplay.js

```
import React, { Component } from 'react'
import Proptest from './Proptest.js';
import axios from 'axios';

class TextDisplay extends Component {

  constructor() {
    super()
    this.state = {
      data: ''
    }
  }
  componentWillMount() {
    axios.get('http://localhost:3000/hi')
    .then((res) => {
      console.log(res.data);
      this.setState({data: res.data});
    })
  }

  render() {
    return (
      <div>
        {this.state.data}
      </div>
    )
  }
}

export default TextDisplay
```

#Stateless component

之前我們寫元件都是使用class寫法
React.js官方說明使用以下方式將比使用class寫法具有更好的效能

Proptest.js
```
import React, { Component } from 'react'

const Proptest = ({name}) => (
  <div>{name}</div>
);

export default Proptest
```

TextDisplay.js

```
import React, { Component } from 'react'
import Proptest from './Proptest.js';
import axios from 'axios';

class TextDisplay extends Component {

  constructor() {
    super()
    this.state = {
      data: ''
    }
  }

  render() {
    return (
      <div>
        <Proptest name="HIHI,this is stateless component" />
      </div>
    )
  }
}

export default TextDisplay
```

其為一個簡單的function具有參數，而在父元件寫入的同名props就是他的參數名稱

>但stateless component不能使用生命週期以及ref