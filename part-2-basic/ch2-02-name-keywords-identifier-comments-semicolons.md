# 2.2 命名、关键字、标识符、注释与分号

## 2.2.1 命名

### 文件名

Go 的源文件以 .go 为后缀名存储在计算机中，这些文件名均由小写字母组成，如 io.go 。如果文件名有多部分，则使用下划线 `_` 对它们进行分隔，如 io_test.go 。文件名不包含空格或其他特殊字符。

>一些建议:

- 尽量避免文件名有多个部分, **test** 文件除外
- 确保文件名简短精确
- 后缀为`xxx_test.go`的文件为`xxx.go`对应的测试文件，请遵循此规则

### 其他命名

Go语言中的包名、常量名、变量名、结构体名、接口名、方法/函数名等所有的命名，均遵循统一的命名规则：

- 首字符可以是任意的Unicode字符或者`_`，一般建议为: `字母`或`_`开头
- 剩余字符可以是`Unicode字符`、`下划线`、`数字`
- 符长度不限(清楚表达意义即可，不建议过长)
- 推荐驼峰命名法
- 包名(package) 小写, 尽量简洁且仅包含单个单词！
- 其他命名区分大小写, 小写不可导出，即仅包内可见!
- 可导出(大写字母开头)的**接口名、结构体名请勿使用包名为前缀命名**!!! _**stutters**_
- 禁止使用Go语言的关键字和保留字命名!

## 2.2.2 关键字

```text
break        default      func         interface    select
case         defer        go           map          struct
chan         else         goto         package      switch
const        fallthrough  if           range        type
continue     for          import       return       var
```

## 2.2.3 标识符

### Go 官方规定的字母与下划线

>character _ (U+005F) 也看作是字母

```text
letter        = unicode_letter | "_" .
decimal_digit = "0" … "9" .
binary_digit  = "0" | "1" .
octal_digit   = "0" … "7" .
hex_digit     = "0" … "9" | "A" … "F" | "a" … "f" .
```

>标识符命名规则

- **identifier = letter { letter | unicode_digit }**
- 关键字不能作为标识符

>有效标识符举例

```text
a
_x9
ThisVariableIsExported
αβ
```

### 预定义标识符

内建常量:

- true, false, iota, nil

内建类型:

- int, int8, int16, int32, int64,
- uint, uint8, uint16, uint32, uint64,
- uintptr, float32, float64, complex64, complex128,
- bool, byte, `rune`, string, `error`

内建函数:

- append, cap, close, complex, copy,
- `delete`, imag, len, make, new,
- panic, print, println, real, recover

## 2.2.4 注释

Go注释: 文件、包、常量、变量、结构类型(结构体与接口)、方法/函数、其他注释。Go注释格式与C类似，支持单行与多行。

- 注释行 以符合 `//` 开头
- 注释块 以符号 `/* 注释内容 */` 包括

通常情况下， 对外公开的包、函数、常量、变量、结构类型等均需要进行注释。代码是否注释完全，可以通过lint工具进行审查。

### 注释规则参考

- 一般可导出/公共的(大写字母开头的)必须添加注释
- 文件注释: 文件开头位置，大写字母开头，一般为版权及文件说明
- 包注释: 包上一行或多行，以Package 开头，后跟包名，如: **Package http ...**，简要描述此包功能用途等
- 函数、常量、变量、及结构体的注释，均以其名称开头，后面紧跟说明
- 常量组或者变量组, 可在组紧挨着的上一行，以大写开头添加说明即可，如果要在组内添加说明则必须按照实际名称开头，后紧跟相关描述
- 参考注释:
  - `compress/zlib/reader.go`
  - `compress/zlib/reader.go`
  - `context/context.go`
  - `database/sql/sql.go`
  - `fmt/doc.go`
  - `go/token/token.go`
  - `go/types/type.go`
  - `io/io.go`
  - `net/http/method.go`
  - `net/mockserver_test.go`
  - `sort/sort.go`

### 其他注释

>一般: 单独一行或多行, 同时和其它代码或者注释之间通过空行隔开

- **[条件编译注释](https://tip.golang.org/pkg/go/build/#hdr-Build_Constraints)**
  - 单独一行或多行，均以// +build 开头，同时和其它代码或者注释之间通过空行隔开
  - 多行之间为 _AND_
  - `// +build linux,386 darwin,!cgo` 条件编译组合结果是：`(linux AND 386) OR (darwin AND (NOT cgo))`
  - `os/stat_unix.go` `// +build aix darwin dragonfly freebsd js,wasm linux netbsd openbsd solaris`
  - [json wrap for standard json & easy-json](https://github.com/xwi88/kit4go/blob/master/json/jsoniter.go)
- [二进制包](https://tip.golang.org/pkg/go/build/#hdr-Binary_Only_Packages)
  - `//go:binary-only-package` 代表代码中直接引用二进制包。二进制包的位于：`$GOPATH/pkg/` 路徑下
  - [go-binary-only-package](https://github.com/tcnksm/go-binary-only-package)
- 代码生成
  - `//go:generate command argument...`
  - 多条命令多行
  - 依赖 `go-tool generate`, `go generate`
- cgo 注释
  - `net/cgo_linux.go`
  - `net/cgo_unix.go`
- 代码工具生成注释, 如：proto 工具生成的文件，会添加禁止编辑修改等注释!
  - `net/http/http.go` 
    - `//go:generate bundle -o=h2_bundle.go -prefix=http2 -tags=!nethttpomithttp2 golang.org/x/net/http2`
  - `sort/sort.go`
    - `//go:generate go run genzfunc.go`
- 其他
  - `//go:noinline`
  - `//go:nosplit`
  - `//go:noescape`
  - `//go:norace`

#### 示例

##### 条件编译注释

```text
//(linux OR darwin) AND 386

// +build linux darwin
// +build 386
```

##### cgo

```go
package test

/*
#include <stdio.h>
#include <stdlib.h>

void pp(char* s) {
	printf("%s", s);
}
*/
import "C"

```

## 2.2.5 [分号](https://golang.org/ref/spec#Semicolons)

>Go 语言使用分号(Semicolons)作为分隔符，编译器会自动识别添加，自行添加可能则会 _Redundant semicolon_

- 自动添加的情况(以下情况对应行的结尾)
  - an identifier
  - an integer, floating-point, imaginary, rune, or string **literal**
  - one of the keywords break, continue, fallthrough, or return
  - one of the operators and punctuation ++, --, ), ], or }
- 不自动添加的情况
  - 允许复杂的语句占据一行，在结束的")"或"}"之前可以省略分号
