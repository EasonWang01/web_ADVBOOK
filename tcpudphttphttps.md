# TCP, UDP

## TCP, UDP, HTTP, HTTPS

網路七層協議 [https://zh.wikipedia.org/wiki/OSI%E6%A8%A1%E5%9E%8B](https://zh.wikipedia.org/wiki/OSI%E6%A8%A1%E5%9E%8B)

TCP的特色在於傳輸資料時，會有握手的過程，以確保雙方身份，所以花的時間多一點。

而UDP的特色在於傳輸資料時，不需要驗 證資料，不保證正確性，發送端不知道數據是否會正確接收，所以速度較快速

一般瀏覽網頁時使用的協議是HTTP與HTTPS，其主要是基於TCP，為TCP往上之發展

## Node.js中的TCP

在node.js主要使用`net`這個核心模組來提供TCP的相關功能，

一般主要是在做與硬體溝通時會使用到

具有TCP中的TCP server與 TCP client的兩種類型

### 實作

1.進入資料夾第10章中的TCP資料夾，執行test1.js來執行TCP server

2.開啟另一個terminal，一樣進入資料夾第10章中的TCP資料夾，執行test2.js

3.結合Repl

將client test2.js改為如下

```text
var net = require('net');

var HOST = 'localhost';
var PORT = 8000;

var client = new net.Socket(); //建立一個新的socket實例
client.connect(PORT, HOST, function() {

    console.log('CONNECTED TO: ' + HOST + ':' + PORT);
    client.write('hello,this is from client!');//發送給server數據


    const repl = require('repl');
    var test = repl.start('請輸入: ').context;
    test.hello = function() {
      client.write('client說了hello!');
    }
    //之後啟動client後輸入hello()
});

client.on('data', function(data) {
    console.log('DATA: ' + data);
});

client.on('close', function() {
    console.log('Connection closed');
});
```

利用 net module 製作 HTTP server

```javascript
const net = require('net');
const server = net.createServer((socket) => {
  socket.on('data', data => {
    console.log(data.toString())
    socket.end(`
HTTP/1.1 200 OK\r\nContent-type: text/html\r\n
<html>
  <head>
    <style>
      body {
        font-size: 20px
      }
    </style>
  </head>
  <body>test</body>
</html>
    `)
  })
}).on('error', (err) => {
  // Handle errors here.
  throw err;
});

// Grab an arbitrary unused port.
server.listen(8124, () => {
  console.log('opened server on', 8124);
});
```

## Node.js中的UDP

主要使用名為`dgram`的核心模組

全名為`UDP / Datagram Sockets`

### 實作

1.進入資料夾第10章中的UDP資料夾，執行test1.js來執行UDP server

2.開啟另一個terminal，一樣進入資料夾第10章中的UDP資料夾，執行test2.js

3.實作UDP廣播機制

UDP具有TCP所沒擁有的技能\(廣播封包\)，可以把封包廣播給區網內的每一台電腦

使用廣播封包時，LAN 上面的每台電腦都會被迫處理這類封包

### UDP廣播 \(需要兩台以上電腦在同一個區網內才可測試\)

接收方\(所有區網上其他電腦所架設的UDP server\)

```text
var udp = require("dgram");
var socket = udp.createSocket('udp4',function(msg){
   console.log(msg.toString())
});
socket.bind(8080);
```

廣播方client

```text
var udp = require("dgram");
var client = udp.createSocket("udp4",function(){});
client.on("listening",function(){
    client.setBroadcast(true);
})
process.stdin.on("data",function(data){
    client.send(data,0,data.length,8080,"255.255.255.255");
});
```

### 但IPv6 不支援廣播，只支援群播（multicasting），所以可將程式碼改為如下

server

```text
var news = [
   "hello",
   "this is multicasting",
];

var dgram = require('dgram'); 
var server = dgram.createSocket("udp4"); 
server.bind();
server.setBroadcast(true)
server.setMulticastTTL(128); //有關TTL 可參考https://nodejs.org/api/dgram.html#dgram_socket_addmembership_multicastaddress_multicastinterface
server.addMembership('230.185.192.108'); 
//可參考如下
//https://nodejs.org/api/dgram.html#dgram_socket_addmembership_multicastaddress_multicastinterface
//https://en.wikipedia.org/wiki/Multicast_address

setInterval(broadcastNew, 3000);

function broadcastNew() {
    var message = new Buffer(news[Math.floor(Math.random()*news.length)]);
    server.send(message, 0, message.length, 8088, "230.185.192.108");     
    console.log("Sent " + message + " to the wire...");
    //server.close();
}
```

client

```text
var PORT = 8088;
var HOST = '192.168.0.102';
var dgram = require('dgram');
var client = dgram.createSocket('udp4');

client.on('listening', function () {
    var address = client.address();
    console.log('UDP Client listening on ' + address.address + ":" + address.port);
    client.setBroadcast(true)
    client.setMulticastTTL(128); 
    client.addMembership('230.185.192.108',HOST);
});

client.on('message', function (message, remote) {   
    console.log('A: Epic Command Received. Preparing Relay.');
    console.log('B: From: ' + remote.address + ':' + remote.port +' - ' + message);
});

client.bind(PORT, HOST);
```

以上參考至[http://stackoverflow.com/questions/14130560/nodejs-udp-multicast-how-to](http://stackoverflow.com/questions/14130560/nodejs-udp-multicast-how-to)

可參考一篇不錯的文章:[http://beej-zhtw.netdpi.net/07-advanced-technology/7-6-broadcast-packet-hello-world](http://beej-zhtw.netdpi.net/07-advanced-technology/7-6-broadcast-packet-hello-world)

