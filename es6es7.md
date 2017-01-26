以下因Node.js版本不一定支援，所以可下載chrome canary並打開devtool輸入程式碼

許多比較新的js功能會在Chrome Canary開放 

載點:https://www.google.com.tw/chrome/browser/canary.html

以下將會介紹比較常用到的幾種方法
#ES6

https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_2015_support_in_Mozilla

全名為`ECMAScript 6.0`或稱`ECMAScript 2015`


#### #const
用來定義常數，定義後不可更改
```
const a = 12;

然後輸入
a = 13;
將會產生錯誤
```

#### #let

一般變數的作用域是以function來區分，在同層function的同名變數如果重複給值，則會覆蓋前項，包含在if內，但如果if的同名變數是用let宣告，則不會影響到if外的變數
```
function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // different variable
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```
let的作用範圍會被侷限在if內，而不會改到外面的同名變數

#### #spread syntax
功能:把Array，以逗點分隔當成多個值

```
function myFunction(x, y, z) { console.log(x) }
var args = [0, 1, 2];
myFunction(...args); //相同於myFunction(1,2,3)
```


#### #Destructuring assignment(解構賦值)

功能:對應兩個Array或Object並把同名的值附上

```
var o = {p: 42, q: true};
var {p: foo, q: bar} = o;
 
console.log(foo); // 42 
console.log(bar); // true  
```
```
let obj = {x: 1, y: 2}; let {x, y} = obj; // x = 1, y = 2
```

```
var a, b;

[a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
```

#### #Arrow function
功能:簡化匿名function寫法，但會失去this屬性


```
//以前寫法
setTimeout(function(){ console.log('okok'),500});
//箭頭寫法
setTimeout(() => console.log('okok'),500);
```

#### #Object.assign()
功能:結合兩個物件

```
var obj = { a: 1 };
var copy = Object.assign({b: 12}, obj);
console.log(copy); 
```

#### #Promise

1.new Promise傳入一個函數，該函數擁有兩個參數

2.這兩個參數均為函數，分別為resolve()和reject()

```
var promise = new Promise(function(resolve, reject) {
  //first execute 

  if (/* 如first execute成功 */){
    resolve(value);//發出resolve
  } else {
    reject(error);發出reject
  }
});
```
4.使用resolve()代表成功，使用reject()代表first execute 
部分沒成功，是否成功是由我們自己去判定寫邏輯

5.promise生成後可用then，執行成功或失敗鎖要執行的東西
```
promise.then(function(value) {
  // success//接收到resolve後會執行
}, function(value) {
  // failure //接收到reject後會執行
});
```

範例：

貼上以下範例，之後把改變`value = 1+2`即可看到console的改變
```
var promise = new Promise(function(resolve, reject) {
//first execute
var value = 1+1;
if (value === 2){
  resolve(value);//發出resolve
} else {
  reject(error);發出reject
}
});

promise
.then(function(value) {
  console.log('function被resolve了!')
})
.catch(function(err) {
  console.log(err)
})
```
#### #Promise.all
把許多promise的函式組成一個Array放入Promise.all內，當裡面每個都`resolve`或其中一個被`reject`時才會執行後面的then方法
```
Promise.all([promises...])
.then(function (posts) {
  // ...
}).catch(function(reason){
  // ...
});
```

#### #for of


```
let iterable = [10, 20, 30];

for (let value of iterable) {
  value += 1;
  console.log(value);
}
```

#### #Generator

功能:也是用來處理異步函式，讓他順序執行
不一樣的地方在於function後的星號，和`yield`

1.
```
function* Fruit() {
  yield 'apple';
  yield 'banana';
  return 'ending';
}

var a = Fruit();

```
之後輸入a.next();







#ES7
https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_Next_support_in_Mozilla

全名為`ECMAScript 7.0`或稱`ECMAScript 2016`

#### #Rest parameters destructuring
功能:動態指定參數個數
```
function fun1(...theArgs) {
console.log(theArgs.length);
}

fun1(); // 0
fun1(5); // 1
fun1(5, 6, 7); // 3
```




#### #Array.includes
功能: 取得Array中是否包含某一個元素
```
var a = [1, 2, 3];
a.includes(2); // true 
a.includes(4); // false
```



#ES8
https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_Next_support_in_Mozilla

全名為`ECMAScript 8.0`或稱`ECMAScript 2017`


>Async與Await為ES8特性，但時常被誤以為是ES7

#### #Async與Await


#### #padEnd
功能：將字串補足位數至與第一個參數相同

```
'abc'.padEnd(10);         // "abc       "
'abc'.padEnd(10, "foo");  // "abcfoofoof"
'abc'.padEnd(6,"123456"); // "abc123"
```

#### #padStart
功能：將字串補足位數至與第一個參數相同，但方向為從頭開始


```
'abc'.padStart(10);         // "       abc"
'abc'.padStart(10, "foo");  // "foofoofabc"
'abc'.padStart(6,"123465"); // "123abc"
```

#### #Object.values
功能:取得所有物件中的值，以Array表示
```
var obj = { foo: "bar", baz: 42 };
console.log(Object.values(obj)); // ['bar', 42]

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.values(obj)); // ['a', 'b', 'c']
```

#### #Object.entries
功能:把物件中的key和value每組分別組成一個Array
```
var obj = { foo: "bar", baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.entries(obj)); // [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ]

```














