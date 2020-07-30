# stream\(流\)

HTTP server 的request 和 process.stdout均為其發展的實例

擁有以下幾種type

```text
Readable - 可讀
Writable - 可寫
Duplex - 可同時讀寫
Transform - 可轉換讀或寫的格式後再輸出
```

一般讀寫有兩種方式

一種是全部先讀取到buffer緩衝區，也就是電腦的記憶體，之後再一次輸出

另一種是另一種是每讀入一小塊隨即將他輸出

