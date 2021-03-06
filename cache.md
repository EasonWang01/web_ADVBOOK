# 快取與緩存

## 快取與緩存

### 簡介:

1.我們在瀏覽器瀏覽圖片或影片時檔案通常會較大，此時如果每次瀏覽都重新去跟server要資料速度會很慢，所以有了緩存機制，讓一些檔案下載後存在我們本地端，此時之後再瀏覽就不用再次去找server要資料

2.而我們也可以利用Redis或memcached來做簡單文字資料的存儲，或是直接把資料存在記憶體中，之後可以快速取用，這兩種都是比直接存在大型資料庫如`MySQL`快速

存取記憶體的方式可參考[https://www.npmjs.com/package/node-cache](https://www.npmjs.com/package/node-cache)

### HTTP Header 識別快取

#### \#If-Modified-Since與Last-Modified

```text
1.第一次訪問不會有If-Modified-Since，只有server回應的Last-Modified
2.之後client請求即會帶有If-Modified-Since時間即為上次server回應的Last-Modified
3.如果時間不一致則會重新去向server要資料，如果相同則不會再去要資料，而是直接返回304
```

![](.gitbook/assets/螢幕快照%202017-02-02%20上午11.28.16.png)

#### \#Etag

```text
server會產生並傳回一個隨機數，通常是檔案內容的hash。用戶端不必瞭解hash是如何產生的，只需要在下一個請求中將其傳送給伺服器：如果hash仍然一致，說明資源未被修改，之後就可以不用再次向server要資料。
```

![](.gitbook/assets/螢幕快照%202017-02-02%20上午11.26.08.png)

#### \#Expires

直接設定某個時間點前都可以保有緩存

![](.gitbook/assets/螢幕快照%202017-02-02%20上午11.57.07.png)

#### \#Cache-Control

```text
"no-cache" 和 "no-store"

「no-cache」表示必須先與伺服器確認傳回的回應是否已變更，然後才能使用該回應來滿足後續對同一個網址的請求。因此，如果存在(ETag)，no-cache 會發起往返通訊來驗證快取的回應，如果資源沒有任何變更，即可避免下載步驟。

相較之下，「no-store」更加簡單，直接禁止瀏覽器和所有中繼快取儲存傳回的任何回應版本，每次使用者請求時，都會向伺服器發送一個請求，而且每次都會下載完整的回應。

「public」和「private」

如果回應標記為「public」，即使具備關聯的 HTTP 認證，甚至回應狀態碼無法正常快取，回應也可以供使用者快取。在大多數情況下，「public」並不是必要項目，因為明確的快取資訊 (例如「max-age」) 已表示 回應可供快取。

另一方面，瀏覽器可以快取「private」回應，但是通常只開放給單一使用者快取，因此不允許任何中繼快取對其進行快取，例如使用者瀏覽器可以快取包含使用者私人資訊的 HTML 網頁，但是 CDN 不能快取。

"max-age"

這個指令指定從目前請求開始，允許擷取的回應重複使用的最長時間 (單位為秒)，例如「max-age=60」表示回應可以再快取及重複使用 60 秒。
```

![](.gitbook/assets/螢幕快照%202017-02-02%20上午11.29.30.png)

## 控制檔案快取

在副檔名後方加上參數，如此一來瀏覽器會認定URL變得不相同，向伺服器請求新的下載

```text
style.css?ai34mc (？號後可隨機輸入)
test.js?cihofew
test1.jpg?123123dfwef
```

## prefetch

```text
<link rel="prefetch" href="image.png">
```

預先存取資源並先存入快取

## CDN

CDN\(Content delivery network\)內容傳遞網路

假設server在台灣．client端瀏覽器在美國，則他可以直接向位於美國某一個州的CDN server存取資源即可，而該美國CDN server會存有與台灣server相同的資料

如此會加快連線速度

![](.gitbook/assets/螢幕快照%202017-02-02%20上午11.53.00.png)

客戶端瀏覽器先檢查本地緩存是否過期，如果過期，則向CDN邊緣節點發起請求，CDN邊緣節點會檢測用戶請求數據的緩存是否過期，如果沒有過期，則直接響應用戶請求，此時一個完成http請求結束；如果數據已經過期，那麼CDN還需要向源站發出回源請求（back to the source request）,來拉取最新的數據

