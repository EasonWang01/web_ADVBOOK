#快取與緩存


##簡介:

1.我們在瀏覽器瀏覽圖片或影片時檔案通常會較大，此時如果每次瀏覽都重新去跟server要資料速度會很慢，所以有了緩存機制，讓一些檔案下載後存在我們本地端，此時之後再瀏覽就不用再次去找server要資料

2.而我們也可以利用Redis或memcached來做簡單文字資料的存儲，或是直接把資料存在記憶體中，之後可以快速取用，這兩種都是比直接存在大型資料庫如`MySQL`快速

存在記憶體的方式可參考https://www.npmjs.com/package/node-cache

##HTTP Header 識別快取

### #If-Modified-Since與Last-Modified

```
1.第一次訪問不會有If-Modified-Since，只有server回應的Last-Modified
2.之後client請求即會帶有If-Modified-Since時間即為上次server回應的Last-Modified
3.如果時間不一致則會重新去向server要資料，如果相同則不會再去要資料，而是直接返回304
```
![](/assets/螢幕快照 2017-02-02 上午11.15.36.png)


### #Etag

```

server會產生並傳回一個隨機數，通常是檔案內容的hash。用戶端不必瞭解hash是如何產生的，只需要在下一個請求中將其傳送給伺服器：如果hash仍然一致，說明資源未被修改，之後就可以不用再次向server要資料。

```
可看上圖的Response Header 的 Etag參考

### #Cache-Control 

