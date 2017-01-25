#HTTP,HTTPS

HTTP是TCP的上層協定，設計HTTP最初的目的是為了提供一種發布和接收HTML頁面的方法。通過HTTP或者HTTPS協定請求的資源由統一資源識別元（Uniform Resource Identifiers，URI）來標識，也就是我們俗稱的網址

通常，由HTTP用戶端發起一個請求(例如在網址列輸入網址)，將會建立一個到伺服器指定埠（預設是80埠）的TCP連線。HTTP伺服器則在那個埠監聽用戶端的請求。一旦收到請求，伺服器會向用戶端返回一個狀態，比如"HTTP/1.1 200 OK"

上面的200，指的是status code，可參考https://zh.wikipedia.org/zh-tw/HTTP%E7%8A%B6%E6%80%81%E7%A0%81

將在後面緩存章節再次提到

#### #HTTP 請求方法
比較常見的是以下幾種

```
GET：向指定的資源發出「顯示」請求。使用GET方法應該只用在讀取資料，而不應當被用於產生「副作用」的操作中，例如在Web Application中。其中一個原因是GET可能會被網路蜘蛛等隨意存取。參見安全方法
POST：向指定資源提交資料，請求伺服器進行處理（例如提交表單或者上傳檔案）。資料被包含在請求本文中。這個請求可能會建立新的資源或修改現有資源，或二者皆有。
PUT：向指定資源位置上傳其最新內容。
DELETE：請求伺服器刪除Request-URI所標識的資源。
```
你可能聽過一個詞 `Restful api`，其中包含對資源的操作包括獲取、創建、修改和刪除資源，這些操作正好對應HTTP協議提供的GET、POST、PUT和DELETE方法。
可參考:https://zh.wikipedia.org/wiki/REST

而傳輸封包中的格式可以是XML或是JSON或是protocol buffer
