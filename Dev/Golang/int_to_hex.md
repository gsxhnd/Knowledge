---
title: Golang从Int转换为十六进制
tags:
  - Golang
created: 2022/09/21 09:36
---

<!-- markdownlint-disable MD025 MD010-->

在 Golang 中，将整数转换为十六进制很容易。因为十六进制实际上只是一个 Integer 字头，你可以使用 fmt.Sprintf()和%x 或%X 格式，向 fmt 包索取该整数的字符串表示。

```go
package main

import (
	"fmt"
)

func main() {
	for i := 1; i < 20; i++ {
		h := fmt.Sprintf("0x%x", i)
		fmt.Println(h)
	}
}
```

转换回 Int

```go
package main

import (
	"fmt"
	"strconv"
)

var hex = []string{"1", "2", "3", "4", "5", "6", "7", "8", "9", "a", "b", "c", "d", "e", "f", "10", "11", "12", "13"}

func main() {
	for _, h := range hex {
		i, _ := strconv.ParseInt(h, 16, 64)
		fmt.Println(i)
	}
}
```
