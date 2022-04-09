# Golang 学习笔记

## 学习资源
### 代理加速
[Goproxy 中国](https://goproxy.cn/) 中国最可靠的 Go 模块代理。
可完美解决由于连接超时无法安装依赖库，工具库的问题。
```
$ go env -w GO111MODULE=on
$ go env -w GOPROXY=https://goproxy.cn,direct
```

国外代理
```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.io,direct
```
### 环境变量
`export GOPATH=/Users/spaceack/Documents/go`

### 1.1 Hello World
gopl_hello.go
```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("Hello world!")
}

```
运行： `go run gopl_hello.go`
编译：`go build gopl_hello.go`
代码格式化工具: `gofmt`
自动添加或删除import 声明：`goimports`
- Go语言的代码通过包（package）组织，包类似于其它语言里的库（libraries）或者模块（modules）。一个包由位于单个目录下的一个或多个.go源代码文件组成, 目录定义包的作用。每个源文件都以一条是`package` 声明语句开始，这个例子里就是`package main` , 表示该文件属于哪个包，紧跟着一系列导入（import）的包，之后是存储在这个文件里的程序语句。
- `main`函数也很特殊，它是整个程序执行时的入口。不包含参数，不返回任何内容，在执行程序时调用。
- 必须告诉编译器源文件需要哪些包，这就是跟随在`package`声明后面的`import`声明扮演的角色。必须恰当导入需要的包，缺少了必要的包或者导入了不需要的包，程序都无法编译通过。
- ` import`声明必须跟在文件的`package`声明之后。
- `func` 创建函数
- `Println()`：此方法存在于 fmt 包中, 打印行。
- 查看golang版本
	`go version`

- 注释
```go
/*
块
注释
*/
// 行注释
```

- 每个 Go 程序都是由包(**package**)组成的。
- 程序入口是 package ```main```

- 惯例: 包名与导入路径的最后一个目录一致

- 打包导入:
```go
import（
  "fmt"
  "math"
)
```

- 包被导入后，可用导出的名称调用它
- 首字母大写的名称是被导出的

### 函数
- 类型在变量名之后
- 多个参数类型一致时，可省略前面的
```go
func add(x int, y, int) int {
  x + y
}
func add(x, y int) int {
   x + y
}
```
- 返回值可以被命名

``` go
func bmi(weight, height float64) (bmi float64, is_normal bool) {
	bmi = weight / (math.Pow(height, 2))
	if (18.5 < bmi) && (bmi < 23.9) {
		is_normal = true
	}
	return
}
```
- ```var```语句可以定一个变量列表
- 函数内可以使用简洁赋值语句声明变量```:=```，替代```var```

### 基本类型
---
	bool
	string
	int  int8  int16  int32  int64
	uint uint8 uint16 uint32 uint64 uintptr
	byte // uint8 的别名
	rune // int32 的别名
		// 代表一个Unicode码
	float32 float64
	complex64 complex128
---
```go
package main

import (
	"fmt"
	"math/cmplx"
)

var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)

func main() {
	const f = "%T(%v)\n"
	fmt.Printf(f, ToBe, ToBe)
	fmt.Printf(f, MaxInt, MaxInt)
	fmt.Printf(f, z, z)
}

```
使用`go build test.go` 编译

- 类型转换，需要显式转换
- 常量 ```const``关键字定义


## 使用iris web框架
run `go mod init test`
- main.go
	```go
	package main
	import "github.com/kataras/iris/v12"
	func main(){
		app ：= iris.New()
		app.Get("/", func(ctx iris.Context){})
		app.Run(iris.Addr(:"8080"))
	}
	```
run `go run main.go`

