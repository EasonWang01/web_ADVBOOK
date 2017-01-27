Post 

>你可能看過$ref: '#/definitions/test1'，之後把test1另外定義，但個人感覺此種寫法比較分散，於是此處不用此種寫法


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