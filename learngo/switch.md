```go
func main() {
    finger := 4
    switch finger {  //或者 finger := 4; finger
    case 1:
        fmt.Println("Thumb")
    case 2:          //case 不能重复
        fmt.Println("Index")
    default:         // 默认情况
        fmt.Println("incorrect finger number")
    }
}
```

## 多表达式判断
```go
func main() {
    letter := "i"
    switch letter {
    case "a", "e", "i", "o", "u": // 一个选项多个表达式
        fmt.Println("vowel")
    default:
        fmt.Println("not a vowel")
    }
}
```
## Fallthrough 语句
匹配到 `case` 后不再跳出 `switch` ，而是继续向下执行运行到下一个 `case`
```go
func main() {
	switch finger := 3; finger {
	case 1, 2:
		fmt.Println("Index")
	case 3:
		fmt.Println("Middle")  //输出
		fallthrough
	case 4:
		fmt.Println("Ring")    //输出后跳出
	case 5:
		fmt.Println("Pinky")
	default: // 默认情况
		fmt.Println("incorrect finger number")
	}
}
```
