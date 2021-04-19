---
title: "Github - 讓 Github Pages 自己動起來"
date: 2020-04-06T00:39:31+08:00
categories: ["github"]
tags: ["git","github"]
draft: true
description: ""
comment: true
---

## 前言

前陣子在練習 React 時，在教學中有教如何上傳到 Github Pages ，他是使用 gh-pages

但是後面其實遇到，修改 Bug 上傳到 Master 後，page 上是沒有更新的

所以這篇要來紀錄如何使用 Github Action 去自動更新 Github Pages 


## 加入檔案

在專案的根目錄中，建立資料夾
 
.github/workflows/main.yml

![Alt text](/images/messageImage_1586169841721.jpg)


## 編輯 Main.yml

```console
#請將此段貼上至Main.yml

 name: Upload Website

on:
  push:
    branches:
      - master
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
        with:
          submodules: true

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.74.1'
          extended: true

      - name: build
        run: hugo -D --gc --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.ACCESS_TOKEN }}
          force_orphan: true
          BRANCH: gh-pages



```

## 建立 Secret

 請在 Github 右上角 點選 Setting
 
```console
 Setting > Developer settings > Personal access tokens 
```

接著點選 Generate new token

![Alt text](/images/messageImage_1586178688405.jpg)

![Alt text](/images/1586179414166.jpg)

切記要記住此組金鑰
他只會顯示一次!!!!

 
## 輸入金鑰

 再來就到專案的 Setting 
 
```console
 Setting > Secrets > Add a new secret 
``` 
![Alt text](/images/messageImage_1586179666346.jpg)

輸入後就完成了

## 後記

做到這邊，全部工作就完成了
往後只要把 Commit 推到 Master 
Action 就會自己更新了



## 參考文章
   - [Github - CICD 使用 Actions 以 React 建置 pages 為例](https://dotblogs.com.tw/explooosion/2019/09/06/152105)
   - [上傳至 Github Pages](https://ithelp.ithome.com.tw/articles/10228423)


 

 
