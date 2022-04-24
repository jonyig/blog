---
title: "Go 的 map 旅程 - hmap"
date: 2022-04-23T17:50:42+08:00
categories: ["golang"]
tags: ["core","go"]
draft: true
comment: true
---

## 版本

1.16

## 描述

基本上我們在使用 Go 的 map 的大致用法會是

```
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

不過這篇會紀錄探究 map 底層的結構

會先介紹 `hmap`

```
// A header for a Go map.
type hmap struct {
	// Note: the format of the hmap is also encoded in cmd/compile/internal/reflectdata/reflect.go.
	// Make sure this stays in sync with the compiler's definition.
	count     int // # live cells == size of map.  Must be first (used by len() builtin)
	flags     uint8
	B         uint8  // log_2 of # of buckets (can hold up to loadFactor * 2^B items)
	noverflow uint16 // approximate number of overflow buckets; see incrnoverflow for details
	hash0     uint32 // hash seed

	buckets    unsafe.Pointer // array of 2^B Buckets. may be nil if count==0.
	oldbuckets unsafe.Pointer // previous bucket array of half the size, non-nil only when growing
	nevacuate  uintptr        // progress counter for evacuation (buckets less than this have been evacuated)

	extra *mapextra // optional fields
}

## https://github.com/golang/go/blob/master/src/runtime/map.go#L115
```

`hmap`  目前還不是主要儲存 key value 的結構

他這邊做的是一些 map 結構的基本設定

現在來一一解說

1. `count` : map 儲存 key 的數量，相當於 `len(m)`
2. `flag` : 目前 map 的操作狀態，用於併發紀錄
3. `B` : 目前 map 內桶子的數量，計算方法為 2^B，基本上最少為兩個桶
4. `noverflow` : 估計 overflow 桶子數量
5. `hash0` : 為 hash 設定一組隨機碼
6. `buckets` : 主角！就是桶子本人，他這邊是 []bmap 格式儲存
7. `oldbuckets` :
8. `nevacuate` : 表示搬移的進度
9. `extra` :

## 相關連結

- [https://github.com/golang/go/blob/master/src/runtime/map.go](https://github.com/golang/go/blob/master/src/runtime/map.go)
- [https://segmentfault.com/a/1190000039101378](https://segmentfault.com/a/1190000039101378)
- [https://www.kevinwu0904.top/blogs/golang-map/](https://www.kevinwu0904.top/blogs/golang-map/)
