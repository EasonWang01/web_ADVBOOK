# 自動化流程與搭建travis ci

## 自動化流程Jenkins CI與搭建Travis CI

## Travis CI

1.在剛才單元測試專案新增`.travis.yml`

```
language: node_js
node_js:
- stable
cache:
  directories:
  - node_modules
branches:
  only:
  - master
```

2.之後把project上傳到github上

新增`.gitignore`

```
node_modules
```

讓node\_modules不要上傳到github上

```
git init
git remote add origin <url>
git add .
git commit -m 'add'
git push origin master
```

3.進入[https://travis-ci.org/](https://travis-ci.org/)

使用github帳號登入 然後點選右上方你的頭像，進入profile

點選右上方的紅色按鈕`sync account`

找到剛上傳的專案，把他的開關打開

4.到剛才程式專案的index.js加入

```
exports.test2 = (num) => {
  return true;
}
```

5.再次上傳

```
git add .
git commit -m 'add'
git push origin master
```

6.回到travis網頁點選重新整理，之後點選剛才專案，即可看到他自動跑測試
