# 2.1 Hello World Run

## 你好，世界

```go
// hello.go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("Hello World!")
}
```

## 运行

```bash
cd examples/ch2
go run hello.go
```

## build

```bash
cd examples/ch2
go build hello.go
./hello

go build -o hello_bin hello.go
./hello_bin
```
