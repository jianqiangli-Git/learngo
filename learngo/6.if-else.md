不同于其他语言，如 C 语言，Go 语言里的 `{ }` 是必要的，即使在 `{ }` 之间只有一条语句。
```go
if condition {  
} else if condition {
} else {
}
```
还有一种 `if` 的形式，在 `if` 中声明变量，该变量只能用于 `if-else` 中
```go
if statement; condition {  
}
```
注意：`else` 语句应该在 `if` 语句的大括号 `}` 之后的同一行中。以下会报错
```go
if num % 2 == 0 { //checks if number is even
    fmt.Println("the number is even") 
}  
else {
    fmt.Println("the number is odd")
}
```
因为 Go 中，如果 `}` 是在最终行，则自动在 `}` 之后插入一个分号。即变成了
```go
if num % 2 == 0 { //checks if number is even
    fmt.Println("the number is even") 
};  //<--自动插入  
else {
    fmt.Println("the number is odd")
}
```

