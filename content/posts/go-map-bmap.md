---
title: "Go 的 map 旅程 - bmap"
date: 2022-04-24T17:50:42+08:00
categories: ["golang"]
tags: ["core","go"]
draft: true
comment: true
---

## 版本

1.16

## 描述

基本上我們在使用 Go 的 map 的大致用法會是

```bash
m := map[int8]int{
		1:1,
		2:2,
		3:3,
	}

v1 := m[1]
v2, ok := m[2]

fmt.Println(v1)
fmt.Println(v2, ok)
```

還記得上一篇紀錄了 `[hmap](https://jonny.website/posts/go-map-hmap/)`

這次會介紹 `bmap`

```bash
// A bucket for a Go map.
type bmap struct {
	// tophash generally contains the top byte of the hash value
	// for each key in this bucket. If tophash[0] < minTopHash,
	// tophash[0] is a bucket evacuation state instead.
	tophash [bucketCnt]uint8
	// Followed by bucketCnt keys and then bucketCnt elems.
	// NOTE: packing all the keys together and then all the elems together makes the
	// code a bit more complicated than alternating key/elem/key/elem/... but it allows
	// us to eliminate padding which would be needed for, e.g., map[int64]int8.
	// Followed by an overflow pointer.
}

## https://github.com/golang/go/blob/master/src/runtime/map.go#L150
```

`bmap`  這邊的結構是初步 hash 的結構

會經由反射在生成一組結構

實際會長這樣

```bash
type bmap struct {
		topbits  [8]uint8
		keys     [8]keytype
		elems    [8]valuetype
		overflow uintptr
		pad      uintptr
}

## https://github.com/golang/go/blob/7677616a263e8ded606cc8297cb67ddc667a876e/src/cmd/compile/internal/gc/reflect.go#L83
```

1. `topbits` : 儲存 hash key 的 前八位
2. `key` : hash key
3. `elems` : value 值
4. `overflow` : 指向下一個 bmap
5. `pad` :

map 的格式長這樣

圖片參考自: [https://www.kevinwu0904.top/blogs/golang-map/](https://www.kevinwu0904.top/blogs/golang-map/)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d1b31b54-f084-496a-95a3-577c3d075876/Untitled.png)

## 相關連結

- [https://github.com/golang/go/blob/master/src/runtime/map.go](https://github.com/golang/go/blob/master/src/runtime/map.go)
- [https://segmentfault.com/a/1190000039101378](https://segmentfault.com/a/1190000039101378)
- [https://www.kevinwu0904.top/blogs/golang-map/](https://www.kevinwu0904.top/blogs/golang-map/)
