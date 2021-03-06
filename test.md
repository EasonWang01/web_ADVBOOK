# 單元測試

有關測試部分

1.新增一個資料夾

2.terminal cd 進入該資料夾 ，執行`npm init`

3.安裝`mocha`與`should`兩個模組

> 注意:mocha要安裝在 global

```text
npm install mocha -g
npm install mocha --save-dev
npm install should --save-dev
```

should的API可參考 [http://shouldjs.github.io/\#assertion-above](http://shouldjs.github.io/#assertion-above)

4.於package.json將script改為如下

```text
"test": "./node_modules/.bin/mocha"
```

我們新增一個index.js

```text
exports.test1 = (num) => {
  return 10 + num;
}
```

5.之後新增test資料夾

裡面放入test.js

```text
var should = require('should');
var testFile = require('../index.js');
var assert = require('assert');

console.log(testFile.test1());

describe('10 + number', function(){
  it('should = 20', function(done){
    var total = testFile.test1(10);
    total.should.equal(20);
    done();
})
  it('should >= 20', function(done){
    var total = testFile.test1(20);
    total.should.be.above(20);
    done();
  })
})
```

然後在專案目錄輸入Mocha即可

他會自動去找專案內的test資料夾內的檔案執行

