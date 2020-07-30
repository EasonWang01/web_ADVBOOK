# EventEmitter\(事件觸發\)

EventEmitter 為Node.js核心模組events下的一個區塊

想像成是一個發送與監聽機制，分別是`emit`與`on`

例如使用者點擊按鈕後會`emit`一個事件名稱，而和`on`綁定的相同事件名之函式則會接收到此事件

