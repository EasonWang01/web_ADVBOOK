# #Restful簡介


一種設計API的風格，而非標準
https://zh.wikipedia.org/wiki/REST

# \#swagger簡介

一個把API說明清楚的方式

swagger有三個服務，editor,codegen,swagger ui

# 使用swagger editor 並開啟localhost cors

因沒開啟的話，位於遠端網頁的 swagger editor 無法發送測試

```
app.use('*', function(req, res, next) {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET, PUT, POST, DELETE, OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Accept, Origin, Content-Type');
    res.header('Access-Control-Allow-Credentials', 'true');
  next();
 });
```

將上面這段加到code的最上面即可


# #開始編寫yaml語言

>yaml子項目會後退空兩格，陣列使用`-`表示

先到http://editor.swagger.io/#/

可以看到範例，接著我們把它清空，開始編寫自己的版本

最基本的型態
```
swagger: '2.0'

info:
  version: 1.0.0
  title: test API
  description: this is test

schemes:
  - http
host: localhost:3000
basePath: /

paths: 
  
```
其中最上面指的是使用swagger的版本，目前固定是2.0.0

中間部分等於是指定API server的位置，意思為http:localhost:3000/

再來我們會開始往paths裡面寫api


GET 簡單範本
```
  /getUser:
    # This is a HTTP operation
    get:
      description: |
        取得使用者，使用query方法
      parameters:
        - name: id
          in: query
          description: 使用者id
          required: true
          type: string       
      responses:
        "200":
          description: Success     
```


>parameters 的 in 可放的參數  "query", "header", "path", "formData" or "body".

>在express中使用req.params取得url中使http://localhost:3000/getUser/123

>使用req.query取得http://localhost:3000/getUser?id=123

GET 方法的完整範例

server.js
```
var express = require('express')
var app = express()

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

app.listen(3000, function () {
  console.log('Example app listening on port 3000!')
})
```

yaml
```

swagger: '2.0'

host: localhost:3000

info:
  version: "1.0.0"
  title: <testAPI>

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
                  
                  
                  
                  
```
  
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