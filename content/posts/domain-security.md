---
title: "解決網域無法轉換 Https 的問題"
date: 2021-04-19T00:39:31+08:00
categories: ["Domain"]
tags: ["domain","blog"]
draft: true
description: ""
comment: true

---
## 前言

前陣子終於把 Blog 建好在 Github pages 上

也把自動更新佈上去 [詳情](https://jonny.website/posts/github-action-cicd/) ，也把我買的 Domain 掛上去

不過卻發生了無法自動轉換 Https 的問題，下面會記錄解決方法



## 前往Cloudflare DNS

1. 新增兩個 A Records，皆指向Github IP：192.30.252.153、192.30.252.154

2. 新增 CNAME，NAME 填 www，Value填上 Github帳號.github.io

## 設定 SSL 憑證 

1. 將加密模式設定為完整 (Full)

2. 將 `一律使用 HTTPS` 設定為 On

## 完成

- 這樣應該就完成了
 


## 參考文章
- [如何使用 Hexo + Github Page 用 Cloudflare 綁定個人網址
  ](https://wualnz.com/%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8-Hexo-Github-Page-%E7%94%A8-Cloudflare-%E7%B6%81%E5%AE%9A%E5%80%8B%E4%BA%BA%E7%B6%B2%E5%9D%80/)
