#TCP,UDP,HTTP,HTTPS

網路七層協議
https://zh.wikipedia.org/wiki/OSI%E6%A8%A1%E5%9E%8B

TCP的特色在於傳輸資料時，會有握手的過程，以確保雙方身份，所以花的時間多一點。

而UDP的特色在於傳輸資料時，不需要驗
證資料，不保證正確性，發送端不知道數據是否會正確接收，所以速度較快速

一般瀏覽網頁時使用的協議是HTTP與HTTPS，其主要是基於TCP，為TCP往上之發展

#Node.js中的TCP

在node.js主要使用`net`這個核心模組來提供TCP的相關功能，