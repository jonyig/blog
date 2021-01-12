---
title: "在 Zeit 中使用 .env 參數"
date: 2020-03-22T00:39:31+08:00
categories: ["Zeit"]
tags: ["NextJs","Zeit","JS","Json"]
draft: true
description: ""
comment: true

---

![Alt text](/images/white-bg-text-logo-1200.png)

這篇文章是給正在使用 Zeit 但是看完官網不了解怎麼使用 Secrets 的人參考

瞭解如何在 Zeit 中使用 .env

## 加入套件

```console
# install now-env package
 $ npm install now-env
```

## 加入參數

```console
 $ now secrets add test-name "my value"
```

## 設定文件

 需要在根目錄新增一個 now.json 的文件
 
 ```console
 {
   "env": {
     "TEST_NAME": "@test-name",
   }
 }
```
 這樣就可以在專案中使用了
 
## 測試方式
 在根目錄中新增一個 now-secrets.json 的文件
 
```console
 {
     "@test-name": "my-test"
 }
```
 這樣就可以正常測試了
 測試完畢推上去 Zeit 即可
 
