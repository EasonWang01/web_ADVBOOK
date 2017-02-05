# #部署

我們接下來要把前幾章實作的網站部署到AWS上，並用nginx當proxy


## 1.使用AWS

AWS 的free Tire有12個月的免費方案，我們將使用此方案

https://aws.amazon.com/

1.之後點選右上角的`sign in`

如果沒有帳號可以先註冊，但他會要求你先填入信用卡帳號，之後他會刷一筆小金額測試，但不會請款

2.登入後先確認右上角的地區是離你最近的地區，之後開機器會在離你近的地區，連線速度會比較快

3.點選左上方的Services選單之後選擇EC2

4.點選Launch Instance

5.我們這裡選擇Ubuntu Server的選項，之後點選右下方next，可把容量設為30G，然後security Group 開啟80 PORT

6.創建新的Key，之後使用此private key連線
>如果告知WARNING: UNPROTECTED PRIVATE KEY FILE!，可使用
chmod 0600 [private key file] 即可

7.之後開啟terminal使用ssh連線，指令類似如下

```
ssh -i ~/Downloads/pem1.pem  ubuntu@ec2-13-112-175-93.ap-northeast-1.compute.amazonaws.com
```

第一次登入需輸入`yes`





##8.安裝nginx


##9.啟動Node.js server 使用pm2模組

##10.設定nginx reverse proxy



##11.加入https，使用Let's encrypt