如果nginx啟動中可參考此篇https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu

將jenkins使用proxy轉到其他port或是把nginx暫停，否則兩個都是80port會無法啟動


##安裝
https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu

此篇環境以Ubuntu為範例

因為jenkins是java寫的，所以要先安裝java環境

### #1.安裝JRE

```
sudo add-apt-repository ppa:openjdk-r/ppa  
```
之後點選Enter

```
sudo apt-get update   
sudo apt-get install openjdk-7-jre  
```

### #2.安裝JDK

```
sudo apt-get install openjdk-7-jdk
```
之後可輸入
```
java -version
```
查看是否正確安裝java

### #3.安裝jenkins

```
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -

sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

sudo apt-get update

sudo apt-get install jenkins
```

### #4.設定jenkins

1.

剛才安裝好後jenkins服務會自動執行，我們直接到`<EC2的IP>:8080`即可

ex:`http://13.112.175.93:8080/`

第一次進去他會要我們輸入一個管理者權限密碼，位置在
`/var/lib/jenkins/secrets/initialAdminPassword`

所以我們到terminal上輸入

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

隨即會顯示該密碼

之後把它貼到jenkins網頁上點選繼續即可

再來點選左邊的`install suggest plugin`安裝推薦的plugin

如果安裝等太久可以直接按重新整理，之後會進入到設定管理者資料的部分

設定完後即可點選`create new jobs `

2.

輸入一個名稱後點選`free style job`之後按左下角ok

之後下拉到`Source Code Management` 把後面加入我們在bitbucket新創立的repo的url 

如果是private的repo記得點選Add 之後輸入你在該網站的帳號密碼

![](/assets/螢幕快照 2017-02-03 上午10.26.00.png)

>注意build之前先確定該repo不是空的

之後我們點選左方的`Build Now`

Build完後點左側的`Workspace`即可發現我們的bitbucket的repo順利被拉近jenkins裡面

![](/assets/螢幕快照 2017-02-03 上午10.41.38.png)

3.

如何讓每次在bitbucket 進行commit時讓jenkins自動build呢？

--安裝bitbucket plugin

點選左側回到dashboard -> 點選左側Manang Jenkins -> 點選中間拼圖形狀的`Manage plugin`->點選上方`Available的Tab`->在右上輸入框輸入`bitbucket`->打勾`Bitbucket Plugin`之後點選下方download and install

--之後點選剛jenkins的專案，點選configuration 之後下拉到`Build Triggers`  將`Build when a change is pushed to BitBucket`打勾


--再來進入bitbucket的repo點選左側齒輪(settings)然後加入webhook

title隨意輸入，URL輸入如下

` JENKINS_URL/bitbucket-hook/ `

EX:`http://13.112.175.93:8080/bitbucket-hook/`

之後隨意更改我們剛才專案的程式碼，再次push到bitbucket即可看到jenkins的build被啟動了

4.

接著我們到configure的Build區塊點選左下的`Add build step`選單，選擇`execute shell`，之後在裡面輸入我們想執行的shell script即可，以後每次jenkins觸發build時都會執行


5.

我們要在jenkins build 時去觸發 自動部署的動作，這裡我們的做法是在execute shell內寫上ssh到web server機器，之後ssh進入後再去執行動作

類似如下圖

![](/assets/螢幕快照 2017-02-04 上午1.16.47.png)



####從jenkins 寫script SSH到別台機器時可能問題

1.`no tty present and no askpass program specified`
https://gist.github.com/hayderimran7/9246dd195f785cf4783d

2.`Host key verification failed`
http://www.51testing.com/html/38/225738-234384.html

3.`Load key "/home/ubuntu/test2.pem": Permission denied
Permission denied (publickey).`

輸入`sudo chmod 777 /home/ubuntu/test2.pem`




