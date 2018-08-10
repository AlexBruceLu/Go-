# TCP状态转换图
![TCP状态转换图](C:\Users\lu_10\Desktop\TCP状态转换图.png)

**主动发起连接请求端：**CLOSED——完成三次握手——ESTABLISHED(数据通信状态)——Dial()函数返回

**被动发起连接请求端：**CLOSED——调用Accept()函数——Listen()——完成三次握手——ESTABLISHED(数据通信状态)——Accept()函数返回数据传递期间——ESTABLISHED(数据通信状态)

**主动关闭连接请求端：**ESTABLISHED(数据通信状态)——FIN_WAIT(半关闭)——TIME_WAIT——2MSL——确认最后一个ACK被对端成功接收。半关闭、TIME_WAIT、2MSL ——只会出现在 “主动关闭连接请求端”

**被动关闭连接请求端：**ESTABLISEHED —— CLOSE

# TCP/CS并发服务器
```go
package main 

import (
    "fmt"
    "net"
    "string"
)

func HandlerConnect(conn net.Conn) {
        defer conn.Close()
        addr := conn.RemoteAddr()
        fmt.Println("Successful client connection!")
        buf := make([]byte,4096)
        // loop read client send data
        for{
            n,err := conn.Read(buf)
            if "exit\n" == string(buf[:n]) || "exit\r\n" == string(buf[:n]) {
                fmt.Println("The server receives a client exit request and the server shuts down!")
                return
            }
            if n == 0 {
                fmt.Println("Server detects client closed to disconnect！！！")
                return
            }
            if err != nil {
                fmt.Println("conn.Read err",err)
                return
            }
            fmt.Println("Server reads data: ",string(buf[:n]))
            // lowercase to uppercase, sent back to client
            conn.Write([]byte(strings.ToUpper(string(buf[:n]))))
        }

}

func main() {
    // create listening socket
    listener,err := net.Listen("tcp",":8008")
    if err != nil {
        fmt.Println("net.Listen err",err)
        return
    }
    defer listener.Close()
    // listening for client connection requests 
    for{
        fmt.Println("the server waits for the client to connect...")
        conn,err := listener.Accept()
        if err != nil {
            fmt.Println("listener.Accept err",err)
            return
        }
        go HandlerConnect(conn)
    }

}

```

# TCP/CS并发客户端

```go
package main 

import (
    "fmt"
    "net"
    "os"
)

func main() {
   // initiating connection request
    conn,err := net.Dial("tcp",":8008")
    if err != nil {
        fmt.Println("net.Dia err",err)
        return
    }
    defer conn.Close()
    // Get the user's keyboard input (stdin) and send the input data to the server 
    go func() {
        str := make([]byte,4096)
        for{
            n,err := coon.Stdin.Read(str)
            if err != nil {
                fmt.Println("coon.Stdin.Read err",err)
                continue
            }
            conn.Write(str[:n])
        }
    }()
    buf := make([]byte, 4096)
    for {
        n, err := conn.Read(buf)
        if n == 0 {
            fmt.Println("Check that the server is shut down and the client shut down")
            return
        }
        if err != nil {
            fmt.Println("conn.Read err:", err)
            return
        }
        fmt.Println("Client reads server postback：", string(buf[:n]))
    }
}

```
