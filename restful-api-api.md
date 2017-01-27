Post 

>你可能看過$ref: '#/definitions/test1'，之後把test1另外定義，但個人感覺此種寫法比較分散，於是此處不用此種寫法

#注意:修改yaml後記得把`Try this operation`重新開啟才會更新

先定義post type

```
consumes:
  - application/x-www-form-urlencoded

```
或是
```
consumes:
  - multipart/form-data
```

--------
路由
```
  /register:         
  
    post:
      description: add a new user
      # movie info to be stored
      consumes:
        - application/x-www-form-urlencoded
      parameters:
        - name: account
          description: 使用者帳號
          type: string
          in: formData
          required: true
        - name: password
          description: 使用者密碼
          type: string
          in: formData
          required: true
      responses:
        "200":
          description: Success        
```

server.js

```
var express = require('express');
var app = express();
var bodyParser = require('body-parser')

app.use(bodyParser());

app.use('*', function(req, res, next) {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET, PUT, POST, DELETE, OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Accept, Origin, Content-Type');
    res.header('Access-Control-Allow-Credentials', 'true');
    next();
});

app.get('/allArticle', function (req, res) {
  res.json({
    result: 'ok',
    data: 'thisisall'
  })
})

app.get('/getArticle/:id', function (req, res) {
  console.log(req.params);
  res.json({
    result: 'ok',
    data: ['data1','data2']
  })
})

app.get('/getUser', function (req, res) {
  console.log(req.query);
  res.json({
    result: 'ok',
    data: ['data1','data2']
  })
})

app.post('/register',function (req,res) {
  console.log(req.body);
  res.json({
    result: 'ok',
    data: ['data1','data2']
  })
})

app.listen(3000, function () {
  console.log('Example app listening on port 3000!')
})
```