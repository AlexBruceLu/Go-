## 第一个go程序

和大多数的语言一样在学习之前,来一个第一个go语言程序:


```go
package main

import "fmt"

func main(){
    fmt.Println("Hello golang!")
}
```

## 变量

**Go 是静态类型语⾔，不能在运⾏期改变变量类型。**
**使⽤关键字 var 定义变量，⾃动初始化为零值。如果提供初始化值，可省略变量类型，由编译器⾃动推断。**


```go
var x int
var f float32 = 1.6
var s = "abc"
```


**在函数内部，可⽤更简略的 ":=" ⽅式定义变量。此方法只能在函数内部,不能定义全局变量.**


```go
func main() {
x := 123 // 注意检查，是定义新局部变量，还是修改全局变量。该⽅式容易造成错误。
}
```

**可⼀次定义多个变量。**


```go
var x, y, z int
var s, n = "abc", 123
var (
a int
b float32
)
func main() {
n, s := 0x1234, "Hello, World!"
println(x, s, n)
}
```


**多变量赋值时，先计算所有相关值，然后再从左到右依次赋值。**


```go
data, i := [3]int{0, 1, 2}, 0
i, data[i] = 2, 100 // (i = 0) -> (i = 2), (data[0] = 100)
```


**特殊只写变量 "_"，⽤于忽略值占位。**


```go
func test() (int, string) {
return 1, "abc"
}
func main() {
_, s := test()
println(s)
}
```


**编译器会将未使⽤的局部变量当做错误。**


```go
var s string // 全局变量没问题。
func main() {
i := 0 // Error: i declared and not used。 (可使⽤ "_ = i" 规避)
}
```


**注意重新赋值与定义新同名变量的区别。**


```go
s := "abc"
println(&s)
s, y := "hello", 20 // 重新赋值: 与前 s 在同⼀层次的代码块中，且有新的变量被定义。
println(&s, y) // 通常函数多返回值 err 会被重复使⽤。
{
s, z := 1000, 30 // 定义新同名变量: 不在同⼀层次代码块。
println(&s, z)
}
```


    输出：
    0x2210230f30
    0x2210230f30 20
    0x2210230f18 30
