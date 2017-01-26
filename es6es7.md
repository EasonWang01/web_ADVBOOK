#ES6

https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_2015_support_in_Mozilla

全名為`ECMAScript 6.0`或稱`ECMAScript 2015`


####Promise

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






#ES7
https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_Next_support_in_Mozilla

全名為`ECMAScript 7.0`或稱`ECMAScript 2016`

#### #Generator


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













