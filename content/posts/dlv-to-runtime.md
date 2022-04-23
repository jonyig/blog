---
title: "Delve runtime 除錯"
date: 2022-04-23T17:50:42+08:00
categories: ["golang"]
tags: ["core","go","tool"]
draft: true
comment: true
---

## 描述

因應在 debug runtime 時，無法輸入一般的 log 模式

因此我們需要來使用 Delve

## 安裝

```bash
go install github.com/go-delve/delve/cmd/dlv@latest
```

## 除錯

可以創建一個範例

```bash
## main

package main

import (
	"fmt"
)

func main() {
	m := map[int8]int{
		1:1,
		2:2,
		3:3,
	}

	v1 := m[3]
	v2, ok := m[2] // ok是一个bool值，用于判断是否存在2这个key

	fmt.Println(v1)
	fmt.Println(v2, ok)
}
```

接下來就執行

```bash
dlv debug main.go

==>

Type 'help' for list of commands.
(dlv)
```

目前只是開啟 dlv 的程式而已，還沒開始跑

需要下中斷點讓他停留

```bash
b main.main
Breakpoint 1 set at 0x10cbc7b for main.main() ./main.go:16
```

接下來就可以直接跑

```bash
c

===>

> main.main() ./main.go:16 (hits goroutine(1):1 total:1) (PC: 0x10cbc7b)
    11: type Person struct {
    12:         Id int `json:"id"`
    13:         Name string `json:"name"`
    14: 
    15: }
=>  16: func main() {
    17:         m := map[int8]int{
    18:                 1:1,
    19:                 2:2,
    20:                 3:3,
    21:         }
(dlv)
```

他就會跑到中斷點停住

這邊筆記一些常用的語法

`s` : 進入函數

`si` : 進入 runtime 函數

`c` : 開始

`stepout` : 跳出函數

`n` : 下一步

`args` : 查看 input 值

`p` : 印出變數

## 重頭戲

要進入這一段的底層函數需要進去 runtime 下中斷點的

```bash
v1 := m[3]

這邊要下 
b runtime.mapaccess1()

才能執行 si
```

因此如果不知道底層呼叫的函數是哪段，就無法進入

## 相關連結

- [https://medium.com/@dubiety/golang-單步除錯利器-delve-7cf4c05e2f08](https://medium.com/@dubiety/golang-%E5%96%AE%E6%AD%A5%E9%99%A4%E9%8C%AF%E5%88%A9%E5%99%A8-delve-7cf4c05e2f08)
