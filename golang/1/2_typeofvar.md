## 内建变量类型

**更明确的数字类型命名，⽀持 Unicode，⽀持常⽤数据结构。**


|类型| ⻓度| 默认值| 说明|
|:---:|:---:|:---:|:---:| 
|bool| 1| false| |
|byte| 1| 0 |uint8|
|rune| 4| 0| Unicode Code Point, int32|
|int, uint |4 或 8| 0 |32 或 64 位|
|int8, uint8| 1| 0 |-128 ~ 127, 0 ~ 255|
|int16, uint16| 2 |0| -32768 ~ 32767, 0 ~ 65535|
|int32, uint32| 4| 0| -21亿 ~ 21 亿, 0 ~ 42 亿|
|int64, uint64| 8| 0| |
|float32| 4| 0.0| |
|float64| 8| 0.0| |
|complex64| 8| | |
|complex128| 16| | |
|uintptr| 4 或 8| | ⾜以存储指针的 uint32 或 uint64 整数|
|array| | | 值类型|
|struct| | | 值类型|
|string| | ""| UTF-8 字符串|
|slice|  |nil |引⽤类型|
|map| | nil| 引⽤类型|
|channel| |nil| 引⽤类型|
|interface| |nil| 接⼝|
|function| |nil| 函数|

***


* complex64(实部和虚部均为float32的复数)/complex128(实部和虚部均为float64的复数)


$$i=\sqrt{-1}$$


$$|3+4i|=\sqrt{3^2+4^2}=5$$


$$i^2=-1,i^3=-i,i^4=1$


***


**欧拉公式**



$$e^{i\pi}+1=0$$



**用go语言实现一下:**


```go
package main

import (
	"fmt"
	"math/cmplx"
	"math"
)

func Euler() {
	fmt.Println(cmplx.Pow(math.E, 1i*math.Pi) + 1)
    //由于浮点数不准确所以精确到三位小数
	fmt.Printf("%.3f\n", cmplx.Exp(1i*math.Pi)+1)
}

func main() {
	Euler()
}
```

**执行结果:**


    (0+1.2246467991473515e-16i)
    (0.000+0.000i)


### 强制类型转换


**go语言里面没有隐式类型转换,只能是显式类型转换:**


```go
package main

import (
	"fmt"
	"math"
)

func triangle()  {
	var a,b int =3,4
	var c int
    //math.Sqrt参数和返回值都是float64
	c=int(math.Sqrt(float64(a*a+b*b)))
	fmt.Println(c)
}

func main(){
    triangler()
}
```


***执行结果:***


    5
