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

```
import React, { Component } from 'react'
import Proptest from "./Proptest"



class TextInput extends Component {

constructor() {
super()
this.state = {
inputText: ' sdst'
}
this.handleChange = this.handleChange.bind(this);
this.deleteLetter = this.deleteLetter.bind(this);
}

handleChange(e){
console.log(e.target.value);

this.setState({inputText:e.target.value});
}
deleteLetter(){
this.setState({
inputText:this.state.inputText.substring(0,this.state.inputText.length-1)
});
}






render() {
var checkFalse = true;
if(checkFalse){
checkFalse = <Proptest text={this.state.inputText} deleteLetter={this.deleteLetter}/>;
}else{
checkFalse = <p>This is false</p>
}



return (
<div>

<input value={this.state.inputText} onChange={this.handleChange} />

{checkFalse}

</div>
)
}

}

export default TextInput
```

使用AJAX

1.先在server.js加上app.post的路徑

```
app.post('/hi',function(req,res){
res.end("hi");
});
```

2.

```
import React, { Component } from 'react'
import Proptest from "./Proptest"



class TextInput extends Component {

constructor() {
super()
this.state = {
inputText: ' sdst'
}
this.handleChange = this.handleChange.bind(this);
this.deleteLetter = this.deleteLetter.bind(this);
}






handleChange(e){
console.log(e.target.value);

this.setState({inputText:e.target.value});
}
deleteLetter(){
$.ajax({
type:"POST",
url: "/hi",
dataType: 'text',
success: function(data) {
console.log(data);
}.bind(this),
error: function(xhr, status, err) {
console.error("error");
}.bind(this)
});


this.setState({
inputText:this.state.inputText.substring(0,this.state.inputText.length-1)
});
}






render() {
var checkFalse = true;
if(checkFalse){
checkFalse = <Proptest text={this.state.inputText} deleteLetter={this.deleteLetter}/>;
}else{
checkFalse = <p>This is false</p>
}



return (
<div>

<input value={this.state.inputText} onChange={this.handleChange} />

{checkFalse}

</div>
)
}

}

export default TextInput
```