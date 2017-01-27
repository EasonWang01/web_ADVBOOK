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

最後加上PUT與DELETE方法

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

app.put('/user', function (req, res) {
  console.log(req.body);  
  console.log('user info updated!')
  res.send('Got a PUT request at /user');
});

app.delete('/user', function (req, res) {
  console.log(req.body);  
  console.log('user deleted!')
  res.send('Got a DELETE request at /user');
});



app.listen(3000, function () {
  console.log('Example app listening on port 3000!')
})
```

yaml

```

swagger: '2.0'

host: localhost:3000
# This is your document metadata
info:
  version: "1.0.0"
  title: <testAPI>

# Describe your paths here
paths:

  /allArticle:
    # This is a HTTP operation
    get:
      # Describe this verb here. Note: you can use markdown
      description: |
        取得所有文章
      responses:
        "200":
          description: Success
                  
  /getArticle/{id}:
    # This is a HTTP operation
    get:
      # Describe this verb here. Note: you can use markdown
      description: |
        取得特定文章使用params方法
      # This is array of GET operation parameters:
      parameters:
        - name: id
          in: path
          description: 文章id
          required: true
          type: string       
      responses:
        "200":
          description: Success
  /getUser:
    # This is a HTTP operation
    get:
      # Describe this verb here. Note: you can use markdown
      description: |
        取得使用者，使用query方法
      # This is array of GET operation parameters:
      parameters:
        - name: id
          in: query
          description: 使用者id
          required: true
          type: string       
      responses:
        "200":
          description: Success  
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
                  
  /user:         
  
    put:
      description: 更新個人資料
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
        - name: info
          description: 個人資料
          type: string
          in: formData
          required: true
      responses:
        "200":
          description: Success                      
    delete:
      description: 刪除使用者
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