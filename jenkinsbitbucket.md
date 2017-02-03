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