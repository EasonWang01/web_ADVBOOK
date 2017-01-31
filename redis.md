# Redis

#### 1.下載

windows下載
https://github.com/dmajkic/redis/downloads

Linux
```
sudo apt insall redis-server
```

Mac using homebrew

https://medium.com/@petehouston/install-and-config-redis-on-mac-os-x-via-homebrew-eb8df9a4f298#.su9r2yd8u


###2.使用

輸入`redis-server`啟動redis

再開一個terminal 輸入`redis-cli`可產生client端

試著在client端輸入 `set food noodle`後`get food`



# #使用Node.js溝通

基本上使用set 與get 即可用來存儲js相關字串或物件及array，但記得要先轉為string，記得要加上第三個參數callback不然可能無法使用

另外操作的過程不用像mongodb一樣寫在ready裡面

```
const redis = require("redis");
const Redisclient = redis.createClient();

export default () => {

  Redisclient.on("ready", function (err) {
      console.log("Ready");
  });

  Redisclient.on("error", function (err) {
      console.log("Error " + err);
  });
}

exports.Redisclient = Redisclient;
```

```
import { Redisclient } from './redis';
import Redis from './redis';
Redis();

const payload = [{a:12,b:13},{a:12,b:13},{a:12,b:13}];

Redisclient.set("short", JSON.stringify(payload), () => {

});
Redisclient.get("short",function (err, reply) {
    console.log(JSON.parse(reply)); // Will print `OK`
});

```

使用Pub與Sub

將test1.js 與 test2.js都改為如下程式碼，之後在terminal輸入訊息，兩邊terminal都會顯示


```
var redis = require("redis");
var sub = redis.createClient(), pub = redis.createClient();

sub.on("message", function (channel, message) {
  console.log(message);
});

process.stdin.on("data",function(data){
  pub.publish("channel001", data);
});

sub.subscribe("channel001");

```
