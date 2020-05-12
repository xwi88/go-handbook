# 1.2 Why Go?

## 为什么要创建 Go?

- C/C++ 的发展速度无法跟上计算机发展的脚步，十多年来也没有出现一门与时代相符的主流系统编程语言，因此人们需要一门新的系统编程语言来弥补这个空缺，尤其是在计算机信息时代。
- 对比计算机性能的提升，软件开发领域不被认为发展地足够快或者比硬件发展更加成功（有许多的项目均以失败告终），同时应用程序的体积始终在不断地扩大，这就迫切地需要一门具备更高层次概念的低级语言来突破现状。
- 在 Go 语言出现之前，开发者们总是面临非常艰难的抉择，究竟是使用执行速度快但是编译速度并不理想的语言（如：C++），还是使用编译速度较快但执行效率不佳的语言（如：.NET、Java），或者说开发难度较低但执行速度一般的动态语言呢？显然，Go 语言在这 3 个条件之间做到了最佳的平衡：快速编译，高效执行，易于开发。

## 目的

- 融合效率、速度和安全的强类型的静态编译语言，同时能够容易的进行编程，让编程变得更有乐趣。
- 较少的关键字和简洁的语法。
- 类型安全和内存安全：在指针类型，但不允许对指针进行操作。
- 支持网络通信、并发控制、并行计算和分布式计算。
- 在语言层面实现对多个处理器（或多核）进行编程。

## Go 特点

- 简化问题，易于学习
- 内存管理，简洁语法，易于使用
- 快速编译，高效开发
- 高效执行
- 并发支持，轻松驾驭
- 静态类型
- 标准类库，规范统一
- 易于部署
- 文档全面，自成文档(通过godoc将代码中的注释信息构造成文档)
- 免费开源(BSD LICENSE)

### 详细特点

- 对Go的并发机制是源于CSP（Communication Sequential Processes），这同样的机制也被于Erlang。
- 对C、C++相比，其语法得到了很大程序上的简化，使代码更简明、清楚，同时拥有动态语言的一些特点，汲取其它语言精华。
- 内嵌运行时反射机制。
- 可以集成C语言实现的库。
- 它不是传统意义上的面向对象语言（没有类的概念），但它有接口（interface），由此实现多态特性。
- 函数（Function）是它的基本构成单元（也可以叫着面向函数的程序设计语言）。
- 是一种静态类型和安全的语言，将其编译、连接成本地代码（拥有高效的执行效率）。
- 支持交叉编译，并采用编译的编码：UTF-8
- 基于BSD完全开源，所以能免费的被任何人用于适合商业目的。
- 语言层面对并发的支持（goroutine：独立于OS的线程，所以多个goroutine可以运行在一个OS的线程里，也可以分布到多个OS线程里。goroutine是从OS线程上抽象出来的一个轻量级的基于CSP的协程。）
    - 在语言层面加入对并发的支持，而不是以库的形式提供
    - 更高层次的并发抽象，而不是直接暴露OS的并发机制
    - 多个goroutine间是并行的
    - 底层混合使用非阻塞IO和线程

![](../images/ch1-02-go-inspired-by.jpg)

图 1-3 其它编程语言对 Go 语言的影响

## 目前缺失特性

- 不支持函数和操作符的重载
- 不支持隐式类型转换，避免产生Bug和迷惑
- 不支持类和继承，可通过 struct 与组合实现
- 不支持动态代码加载
- 不支持动态链接库
- 不支持泛型(有望在2.0添加此特性)

## Go 优秀应用

- 云计算基础设施领域: 
    - [Docker](https://github.com/docker/docker-ce)
    - [K8S](https://github.com/kubernetes/kubernetes)
    - [Etcd](https://github.com/etcd-io/etcd) 
    - [Consul](https://github.com/hashicorp/consul)
    - Cloudflare CDN
    - [Moby](https://github.com/moby/moby)
- 基础软件: 
    - [nsq](https://github.com/nsqio/nsq)
    - [TiDB](https://github.com/pingcap/tidb)
    - [InfluxDB](https://github.com/influxdata/influxdb)
    - [Prometheus](https://github.com/prometheus/prometheus)
    - [Grafana](https://github.com/grafana/grafana)
    - [CockRoachDB](https://github.com/cockroachdb/cockroach)
    - [frp](https://github.com/fatedier/frp)
    - [syncthing](https://github.com/syncthing/syncthing)
    - [traefik](https://github.com/containous/traefik)
    - [goreplay](https://github.com/buger/goreplay)
    - [cayley](https://github.com/cayleygraph/cayley)
    - [dgraph](https://github.com/dgraph-io/dgraph)
    - [bolt](https://github.com/boltdb/bolt)
- 互联网基础设施: 
    - [Ethereum](https://github.com/ethereum/go-ethereum)

## 使用 Go 的公司

- 国外: Google、Docker、Apple、Cloud Foundry、CloudFlare、Couchbase、CoreOS、Dropbox、MongoDB、AWS、Uber等。
- 国内: 字节跳动、哔哩哔哩、阿里云CDN、百度、小米、360、美团、滴滴、搜狗、七牛、PingCAP、华为、金山软件、猎豹移动、饿了么等。

## 学习Go语言的前景

目前Go语言已经⼴泛应用于人工智能、云计算开发、容器虚拟化、⼤数据开发、数据分析及科学计算、运维开发、爬虫开发、游戏开发等领域。

Go语言简单易学，天生支持并发，完美契合当下高并发的互联网生态。
