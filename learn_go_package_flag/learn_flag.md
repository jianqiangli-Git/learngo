resource: [flag包](https://pkg.go.dev/flag)

`flag` 包实现了命令行标志解析

`import "flag"`

使用`flag.String()`, `Bool()`, `Int()`等函数注册flag，下例声明了一个整数flag，解析结果保存在`*int`指针`ip`里：

```go
import "flag"
var ip = flag.Int("flagname", 1234, "help message for flagname")
```
也可以将flag绑定到一个变量，使用`Var`系列函数：
```go
var flagvar int
func init() {
	flag.IntVar(&flagvar, "flagname", 1234, "help message for flagname")
}
```
或者你可以自定义一个用于flag的类型（满足Value接口）并将该类型用于flag解析，如下：
```go
flag.Var(&flagVal, "name", "help message for flagname")
```
对这种flag，默认值就是该变量的初始值。

在所有flag都定义之后，调用：
```go
flag.Parse()
```
来解析命令行参数写入注册的flag里。

解析之后，flag的值可以直接使用。如果你使用的是flag自身，它们是指针；如果你绑定到了某个变量，它们是值。
```go
fmt.Println("ip has value ", *ip)
fmt.Println("flagvar has value ", flagvar)
```
解析后，flag后面的参数可以从`flag.Args()`里获取或用`flag.Arg(i)`单独获取。这些参数的索引为从`0`到`flag.NArg()-1`。

命令行flag语法：
```go
-flag
-flag=x
-flag x  // 只有非bool类型的flag可以
```
可以使用1个或2个'-'号，效果是一样的。最后一种格式不能用于`bool`类型的flag，因为如果有文件名为`0`、`false`等时,如下命令：
```go
cmd -x *
```
其含义会改变。你必须使用`-flag=false`格式来关闭一个`bool`类型flag。

Flag解析在第一个非flag参数（单个"-"不是flag参数）之前停止，或者在终止符"--"之后停止。

整数flag接受1234、0664、0x1234等类型，也可以是负数。`bool`类型flag可以是：
```
1, 0, t, f, T, F, true, false, TRUE, FALSE, True, False
```
时间段flag接受任何合法的可提供给`time.ParseDuration`的输入。
 ## 用法 expmle
 ```go
 package main

import (
	"flag"
	"fmt"
	"strings"
)

/*
将命令行参数绑定到 flag
func flag.String(name string, value string, usage string) *string
依次传入  flag 名字，默认值，用法说明
*/
func first() {
	myName := flag.String("name", "jack", "set name arg") // 将传递给命令行参数flag（name）的值赋给 myName,他是 *String 类型
	flag.Parse()
	fmt.Println(*myName) // 获取地址中的值需要解指针
}

/*
将命令行参数值绑定到变量
func flag.IntVar(p *int, name string, value int, usage string)
依次传入 要绑定的变量地址,flag 名字，默认值，使用说明
*/
func second() {
	var myAge int
	flag.IntVar(&myAge, "age", 13, "set age arg") // 将传递给命令行参数flag（age）的绑定到变量 myAge
	flag.Parse()
	fmt.Println(myAge) // 直接使用变量而不用解指针
}

/*
将命令行参数绑定到结构体
func flag.Var(value flag.Value, name string, usage string)
需要实现 value 接口

	type Value interface {
		String() string
		Set(string) error
	}

其中String方法格式化该类型的值，flag.Parse方法在执行时遇到自定义类型的选项会将选项值作为参数调用该类型变量的Set方法

依次传入 实现了flag.Value接口的结构体，flag 名字，使用说明
*/

type Hobby []string

func (h *Hobby) String() string {
	return strings.Join(*h, ",") // 将传入的字符串加入逗号进行分隔，返回加入逗号后拼接的字符串
}

func (h *Hobby) Set(s string) error { // 定义将传入的字符串 s 如何追加到 Hobby结构体中
	for _, v := range strings.Split(s, ",") {
		*h = append(*h, v) // 此处将传入的字符串以逗号分隔，追加到Hobby结构体中
	}
	return nil
}

func third() {
	var h Hobby
	flag.Var(&h, "hobby", "set hobby arg")
	flag.Parse()
	fmt.Println(h)
}

func test() { // 学到一个新的知识点
	h := func() { // go 中函数里面不能定义函数，只能定义变量。
		fmt.Println("hi")
	}
	h()
}

func main() {
	// first() //go run main.go -name="rose" -> rose
	// second()  //go run main.go -age=10 -> 10
	third() // go run main.go -hobby="play game,football" -> [play game football]，这是首先将hobby加入逗号拼接，返回play game,football，再以逗号切分，返回play game football再append到Hobby结构体即string切片中
	// test()   // -> hi
}
```
