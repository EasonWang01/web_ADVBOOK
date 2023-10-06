# React基本概念1-2(新增元件)

### 第二階段

開始新增其他react元件

在components下，新增一個檔案 `TextDisplay.js`

```
import React, {Component} from 'react'

class TextDisplay extends Component{

}

export default TextDisplay
```

上面是引用react後建造一個空的class後將他輸出

接著我們要在class內寫入東西

```
import React, {Component} from 'react'

class TextDisplay extends Component{

render() {
  return (
    <div>
      <div> 提醒:</div>
      <div> React.js最外層只能有一個div</div>
    </div>
  )};
}

export default TextDisplay
```

接著在App.js中引用他

```
import React, { Component } from 'react';
import TextDisplay from './TextDisplay';

class App extends Component {
  render() {
    return (
      <div>
        <div>
           Hello
        </div>
        <TextDisplay/>
      </div>
    )
  }
}

export default App
```

> 試著將TextDisplay.js最外層的div刪掉，會出現錯誤，因為一個元件要有東西包住最外層。

#### 使用state

TextDisplay.js

```
import React, { Component } from 'react'

class TextDisplay extends Component {

  constructor() {
    super()
    this.state = {
      inputText: 'this is apple'
    }
  }

  render() {
    return (
      <div>
        <input
          type="text"
          placeholder="This is going to be text"
          value={this.state.inputText} />
      </div>
    )
  }

}

export default TextDisplay
```

#### !!記得重新整理網頁，才會作用(因為這裡是constructor)

再來點選網頁中的input框發現沒辦法更改，原因是我們在input的prop中寫的是value把它改為defaultValue即可

## 為元件加入方法

```
import React, { Component } from 'react'

class TextDisplay extends Component {

  constructor() {
    super()
    this.state = {
      inputText: ' sdsxt'
    }
  }

  handleChange() {
    this.setState({inputText: 'hello'})
  }

  render() {
    return (
      <div>
      <input
        type="text"
        placeholder="This is going to be text"
        value={this.state.inputText}
        onChange={() => this.handleChange()} />
      </div>
    )
  }
}

export default TextDisplay
```

接著隨意輸入一個，輸入框都會變為hello字樣

### 在class中的方法如果有this的話他會不知道this是什麼，所以要在class 的constructor中把該方法綁進來

> 記得在constructor使用super()後才可用this

1\.

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

2.另一種寫法，是直接在DOM 的onchange中使用bind

```
onChange={this.handleChange.bind(this)}
```

接著在render上面寫

```
handleChange(){
  ...
}
```

3.第三種寫法(ES6的箭頭函數，最方便，因為會直接幫你綁定)

```
handleSubmit() {

}

<button onClick={()=>this.handleSubmit()}>Submit</button>
```

另外如要傳入事件`e`，記得兩邊()都要寫

```
<button onSubmit={(e)=>this.handleSubmit(e)}>
```

好處是不用再用bind

參考:[http://egorsmirnov.me/2015/08/16/react-and-es6-part3.html](http://egorsmirnov.me/2015/08/16/react-and-es6-part3.html)

## 在class內所有的this都是指到那個class

所以要取得onchange時input內的value必須用e.target ，因為這裡不是DOM

貼上下面程式碼後，在輸入框中輸入，之後觀察console

```
import React, { Component } from 'react'

class TextDisplay extends Component {

  constructor() {
    super()
    this.state = {
      inputText: 'sdsxt'
    }
  }

  handleChange(e) {
    this.setState({inputText: 'hello'})
    console.log(this);
    console.log(e.target);
  }

  render() {
    return (
      <div>
      <input
        type="text"
        placeholder="This is going to be text"
        value={this.state.inputText}
        onChange={(e) => this.handleChange(e)} />
      </div>
    )
  }
}

export default TextDisplay
```
