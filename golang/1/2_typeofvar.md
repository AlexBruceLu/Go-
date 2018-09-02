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
