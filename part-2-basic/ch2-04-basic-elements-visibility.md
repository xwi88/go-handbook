# 2.4 Go 程序的基本组成与可见性

Go程序通常有这些部分组成: *注释*、**包**、_常量_、_变量_、_类型定义_、_接口_、_结构体_、_方法_、_函数_、_语句 & 表达式_。

其中，`必需有的是包`，`建议添加的是注释`，一般一个Go源码文件中至少还会存在函数或者方法。

以下内容示例见: [structure_elements.go](../examples/ch2/structure_elements.go)

## 注释

正常情况下每个文件必须添加**文件注释**(版权信息说明)，**包注释**(用于说明当前包的功能用途等)，其他可导出类型的注释等。更多注释介绍请点击[查看](ch2-02-name-keywords-identifier-comments-semicolons.md)

## 包

每个go文件必须具有包名，用于说明包及文件功能用途。一个可执行的文件中必须存在`main`包, 即 `package main`。

一个go文件如果依赖了其他包则需要使用`import`进行包的导入。未使用的包不允许导入，如果确实需要导入或者留待使用则可以使用 `import _ fmt`，
请注意`_`(空白标识符)，可屏蔽未使用(unused)错误，默认会调用导入包文件的 `init()` 方法，可参考[go-sql-driver](https://github.com/go-sql-driver/mysql#usageo
)的初始化。

>` 默认导入调用了`github.com/go-sql-driver/mysql/driver.go`中的`func init()`

```go
// Package mysql provides a MySQL driver for Go's database/sql package.
//
// The driver should be used via the database/sql package:
//
//  import "database/sql"
//  import _ "github.com/go-sql-driver/mysql"
//
//  db, err := sql.Open("mysql", "user:password@/dbname")
//
// See https://github.com/go-sql-driver/mysql#usage for details
package mysql

```

>不建议的包导入示例

```go
package main

import (
	"fmt"
	_ "fmt"  // Bad
	f1 "fmt" // Bad
	f2 "fmt" // Bad
)

```

**包相关的一些建议**:

- 包命名言简意赅，小写字母(如无特殊情况尽量一个单词，多个单词请使用下划线`_`连接)，遵照Go规范
- 只导入需要使用的包，包含包的初始化导入 `init()`
- 包导入需要遵循一定规则，建议使用 `goimports` 进行导入控制
  - 如果仅仅导入了一个包，可以考虑单行导入放置，如: `import "fmt"`
  - 多包导入，使用 import 分组/块导入，即 `import ( ... )`，多个包在`()`中多行字典序排列
  - 导入包请按照规则分组字典序放置: `标准库包`, `本地包(不建议)`，`golang包`，`初始化包`，`github` 等
- 包重命名, 如果确实需要重命名或者包名冲突，可以进行导入包重命名 `import ft "fmt"`，请避免对系统包重命名!
- 请合理规划调整包结构，避免循环依赖(**cycle import**)出现!
- 特殊的包，`go get` 默认会忽略的
  - `testdata` 用于存放测试即测试数据

## 常量

常量是一个简单值的标识符，在程序运行时，不会被修改的量。

常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。

函数或者方法之外的常量为**全局常量**，内部的为**局部常量**。

_常量的定义格式_：`const identifier [type] = value`

## 变量

常量类似也有`全局`与`局部`之分。

声明变量的一般形式是使用`var`关键字：`var identifier type`

## 类型定义

Go语言中存在一个关键字 `type`，`type` 又有两种使用方式，一种是`类型别名`，一种是`类型定义`。

类型定义是完全定义了一种新的类型，而类型别名只是**给现有的类型取了一个别名** `alias`。带`=`的为类型别名。

```go
package main

type defineInt int
type aliasInt = int
```

`类型定义`类似于C语言中的 `typedef`，`类型别名`相当于*编译期会预处理替换*为int，类似于C语言中的`#define`。

_两个类型内存布局完全一样，但是从概念上说，他们是完全不同的两个类型，不能相互兼容。_

>类型别名这个功能非常有用，可以让我们简化复杂类型命名，编译期替换，不影响性能。

- `type MapStrInt = map[string]int`
- `type MapStrInterface = map[string]interface{}`

## 接口

接口是共性方法的集合，考虑复用性，功能越简单越好，复杂功能可以借助多个接口实现。Go中接口的实现与定义完全分离。只要一个结构类型实现了接口定义的全部的方法，则此结构就是接口的实现。

## 结构体

结构体是一系列相同类型或不同类型的数据构成的数据集合。Go中无继承，可以借助结构体的组合实现类似 `Java` 的继承。

## 函数与方法

_方法是一种特殊的函数，定义在某一特定的类型上，通过类型的实例来进行调用_，这个实例被叫**接收者(receiver)**。接收者必须有一个显式的名字，且这个名字必须在方法中被使用。

- 函数将变量作为参数: `Function(recv receiver_type)`
- 方法被调用: `recv.Method()`

receiver_type 叫做(接收者)类型，这个类型必须在和方法同样的包中被声明。

Go语言不允许为简单内置类型添加方法，否则报错: `Cannot define new methods on non-local type 'builtin.xxx'`

## 可见性

Go的可见性**以包来区分**，并遵循一定规则:

- **小写开头的均表示不可导出，即包内可见**，外部不能访问。_结构体中的一些以小写开头的字段，在序列化时默认会忽略。_
- **凡是大写开头的均表示可导出，即包外可见(访问)**。
- 引入项目中名为`internal`的包，不能被项目引入使用!
