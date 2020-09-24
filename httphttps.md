# HTTP, HTTPS

## HTTP, HTTPS

HTTP是TCP的上層協定，設計HTTP最初的目的是為了提供一種發布和接收HTML頁面的方法。通過HTTP或者HTTPS協定請求的資源由統一資源識別元（Uniform Resource Identifiers，URI）來標識，也就是我們俗稱的網址

通常，由HTTP用戶端發起一個請求\(例如在網址列輸入網址\)，將會建立一個到伺服器指定埠（預設是80埠）的TCP連線。HTTP伺服器則在那個埠監聽用戶端的請求。一旦收到請求，伺服器會向用戶端返回一個狀態，比如"HTTP/1.1 200 OK"

上面的200，指的是status code，可參考[https://zh.wikipedia.org/zh-tw/HTTP%E7%8A%B6%E6%80%81%E7%A0%81](https://zh.wikipedia.org/zh-tw/HTTP%E7%8A%B6%E6%80%81%E7%A0%81)

以下列出常見的Status Code

### HTTP 請求方法

比較常見的是以下幾種

```text
GET：向指定的資源發出「顯示」請求。使用GET方法應該只用在讀取資料，而不應當被用於產生「副作用」的操作中，例如在Web Application中。其中一個原因是GET可能會被網路蜘蛛等隨意存取。參見安全方法
POST：向指定資源提交資料，請求伺服器進行處理（例如提交表單或者上傳檔案）。資料被包含在請求本文中。這個請求可能會建立新的資源或修改現有資源，或二者皆有。
PUT：向指定資源位置上傳其最新內容。
DELETE：請求伺服器刪除Request-URI所標識的資源。
```

`Restful api`，其中包含對資源的操作包括獲取、創建、修改和刪除資源，這些操作正好對應HTTP協議提供的GET、POST、PUT和DELETE方法。 可參考:[https://zh.wikipedia.org/wiki/REST](https://zh.wikipedia.org/wiki/REST)

而傳輸封包中的資料格式可以是XML或是JSON或是年代算比較新的protocol buffer

### 你可能也聽過SPDY或是HTTP2一詞

可參考

[https://ye11ow.gitbooks.io/http2-explained/content/part11.html](https://ye11ow.gitbooks.io/http2-explained/content/part11.html)

[https://http2.github.io/faq/](https://http2.github.io/faq/)

主要是拿來改善原本的HTTP/1.1的幾個問題，HTTP2的優點包含

```text
1.在單一網路連線上，可以同時傳輸多個 HTTP Request 與 Response
2.傳輸封包Header的壓縮，因為在 HTTP/1.1 的Headers是沒有壓縮的
3.Binary的封包結構
4.伺服器主動推送資源
```

## HTTP實作

參考程式碼HTTP與HTTPS資料夾

