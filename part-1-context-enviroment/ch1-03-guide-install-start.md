# 1.3 GO 安装及环境配置

## Install

- [download pkg](https://golang.org/dl/)
- [install guide](https://golang.org/doc/install)

### Mac Install

> [brew ref](https://docs.brew.sh/FAQ)

- `brew update`
- `brew install go@1.14`

```bash
#default auto generate!
GOPATH="$HOME/go"
GOROOT="/usr/local/Cellar/go/1.14.2_1/libexec"
GOTOOLDIR="/usr/local/Cellar/go/1.14.2_1/libexec/pkg/tool/darwin_amd64"
```

### Tarballs Install

```bash
tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz

#Add /usr/local/go/bin to the PATH environment variable.
export PATH=$PATH:/usr/local/go/bin
```

### Installing extra Go versions

```bash
go get golang.org/dl/go1.14.1
go1.14.1 download
```

### Uninstalling Go

>remove the bin file and reset the related ENV set

## Env Configs

```bash
#enable go module, go1.14 auto enable, no need set!
#export GO111MODULE=on

#use go module, no need set GOPATH
#export GOPATH=$HOME/go
#export PATH=$PATH:$GOPATH

#go bin
export GOBINARY=$HOME/go/bin
export PATH=$PATH:$GOBINARY

# go proxy config
# export GOPROXY=https://goproxy.cn,https://goproxy.io,https://proxy.golang.org,direct
GOPROXY=https://mirrors.aliyun.com/goproxy,https://goproxy.cn,https://goproxy.io,https://gocenter.io,https://proxy.golang.org,direct
```

## Hello World

>[代码查看](../examples/ch1/helloworld.go)

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("Hello World！")
}
```

## Dirs for Go SDK

```text
┌── api/    go 不同版本 api 说明
├── bin/    包含可执行文件，如：编译器，Go 工具:gofmt, etc.
├── doc/    包含示例程序，代码工具，本地文档等
├── favicon.ico
├── go1.14.2.darwin-amd64.tar.gz
├── lib/    lib 文件，当前有 time 时区文件
├── misc/   包含与支持 Go 编辑器有关的配置文件以及 cgo 的示例
├── pkg/    依赖库支持及工具
├── robots.txt
├── src/    **包含源代码构建脚本和标准库的包的完整源代码**
    |__ cmd/    **包含 Go 和 C 的编译器和命令行脚本**
└── test/   go自测试相关文件
```

## Glossary

- **GOROOT**: go 安装位置, 安装后生成或配置
  - src: contains Go source files.
  - bin: contains the binary executables.
    - go
    - godoc
    - gofmt
  - pkg: contains Go package archives (.a). All the non-executable packages (shared libraries) are stored in this directory.
    - tool
    - include
    - darwin_amd64
    - darwin_amd64_race
- **GOPATH**: go1.11 前的默认工作空间，Unix 默认位置 $HOME/go, Windows 默认 %USERPROFILE%\go。go module 下源码无需放置 GOPATH 下。
  - 可配置在文件: ~/.bash_profile or ~/.zshrc
  - 包含目录
    - bin: go get/install 二进制存放位置
    - pkg: 依赖包安装位置
    - 禁止把GOPATH设置成go的安装路径, 默认路径即可!
- **module**: go1.11 推出， go1.13 默认开启
- `go install`: 项目二进制程序安装
  - **GOBIN**
  - **$GOPATH/bin or $HOME/go/bin**
  - *$GOROOT 下的可执行文件在 $GOROOT/bin or $GOTOOLDIR*
  - disabled module, 依赖安装: $GOPATH/pkg/$GOOS_$GOARCH
  - ***enable module, 依赖只是 build and cache, 不安装!***
- `vendor`: 项目依赖存放目录，需借助包管理工具。go module 功能不需要配置此目录。
