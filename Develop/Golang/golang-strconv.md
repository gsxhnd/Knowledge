---
title: Golang类型转换方法(strconv包)
tags:
  - Golang
date: 2020/05/26 13:29
---

## int 转化其他类型

```go
import "strconv"  //先导入strconv包

// int到string
i := 10
s := strconv.Itoa(i)
fmt.Printf("%T, %v\n", s, s)
//Output:
//
//string, 10


// int64到string
str := strconv.FormatInt(int64,10)
```

## float 转化其他类型

```go
import "strconv"  //先导入strconv包

//float 转 string
str := strconv.FormatFloat(float32, 'E', -1, 32)

str := strconv.FormatFloat(float64, 'E', -1, 64)
// 'b' (-ddddp±ddd，二进制指数)
// 'e' (-d.dddde±dd，十进制指数)
// 'E' (-d.ddddE±dd，十进制指数)
// 'f' (-ddd.dddd，没有指数)
// 'g' ('e':大指数，'f':其它情况)
// 'G' ('E':大指数，'f':其它情况)
```

## string 转化其他类型

```go
import "strconv"  //先导入strconv包

// string到int
i, err := strconv.Atoi(str)

// string到int64
i64, err := strconv.ParseInt(str, 10, 64)

//string到float32(float64)
f, err := strconv.ParseFloat(str,32/64)

//string 到 uint64(uint32)
ui64, err :=strconv.ParseUint(str,10,64)
```

## uint 转化其他类型

```go
// uint to float64
float64 := float64(uint)
```
