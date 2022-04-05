---
title: "Distributed Locks"
date: 2022-04-05T17:50:42+08:00
categories: ["redis"]
tags: ["redis","lock"]
draft: true
comment: true
---

## 描述

1. API 剛進來先嘗試取得 lock
    1. 如果有取得則開啟看門狗，往 <b>2</b>
    2. 無取得則繼續等待，嘗試取得
2. 執行邏輯
3. 解鎖，關閉看門狗

## 注意點

- 如果 redis 操作分為兩步驟或以上，請使用 lua 保持操作一制性

## 流程圖

{{< mermaid >}}
flowchart TD
	subgraph main [main]
		A(API) --> B(redis)
		B --> C(logical)
		C --> D(return )
	end
	subgraph redis [redis]
		B -->|try to lock| F
		F --> H(condition)
		H -->|try to lock| F
		H -->|if return 1| B
		C <--> E(redis:unlock)
	end
		H -->|when lock, open watch dog| I(watch dog) 
		E -->|when unlock, close watch dog| I
{{< /mermaid >}}



## 相關連結

- [https://www.modb.pro/db/77169](https://www.modb.pro/db/77169)
- [https://juejin.cn/post/7024307688196538375](https://juejin.cn/post/7024307688196538375)
- [https://segmentfault.com/a/1190000038988087](https://segmentfault.com/a/1190000038988087)
- [https://github.com/jonyig/go-leetcode/blob/master/playground/distributed-locks/service/redisRepository.go](https://github.com/jonyig/go-leetcode/blob/master/playground/distributed-locks/service/redisRepository.go)
