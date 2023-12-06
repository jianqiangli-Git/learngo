resource: [flag包](https://pkg.go.dev/flag)

`flag` 包实现了命令行标志解析

## 用法
使用 `flag.String()`,` Bool()`, `Int()`等定义 flags，这声明了一个整形 flag `-n`
```go
import "flag"
var nFlag = flag.Int("n", 1234, "help message for flag n")
```
也可以使用 `Var()` 函数把 flag 绑定到一个变量
```go
var flagvar int
func init() {
	flag.IntVar(&flagvar, "flagname", 1234, "help message for flagname")  //注意这里传递的是 flagvar 的地址
}
```


