# Redis

#### 1.下載

windows下載
https://github.com/dmajkic/redis/downloads

Linux
```
sudo apt install redis-server
```

Mac using homebrew

https://medium.com/@petehouston/install-and-config-redis-on-mac-os-x-via-homebrew-eb8df9a4f298#.su9r2yd8u


###2.使用

輸入`redis-server`啟動redis

再開一個terminal 輸入`redis-cli`可產生client端

###3.Redis儲存類型

```
Redis 字串(String)
Redis 哈希(Hash)
Redis 列表(List)
Redis 集合(Set)
Redis 有序集合(sorted set)
Redis 發布與訂閱(Pub Sub)

```
####1.Redis 字串(String)
(簡單的設定key與value)

```
試著在client端輸入 `set food noodle`後`get food`
```

####2.Redis 哈希(Hash)
(類似於javascript的object)

設定
```
HMSET website google www.google.com yahoo www.yahoo.com
```

取得

```
HGET website google

HGET website yahoo
```

####3.Redis 列表(List)
(類似於Array)

設定

```
LPUSH fruits apple

LPUSH fruits banana
```


取得

0 -1 代表從哪個位置取到哪個位置(-1 代表最尾端)
```
LRANGE fruits 0 -1
```

(LPUSH代表從左端推入也可用RPUSH從右端推入)
(另外也有POP方法可推出元素)

####4.Redis 集合(Set)

常用來計算交集，聯集與差集

1.新增

```
SADD class1 "Eason" "Tim" "Jack"

SADD class2 "Eason" "John" "Peter"
```

2.計算交集

```
SINTER class1 class2
```

3.計算聯集

```
SUNION class1 class2

```

4.計算差集

```
SDIFF class1 class2

```


https://redis.readthedocs.io/en/2.4/list.html

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
