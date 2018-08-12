
# web工作方式
## 1.客户端——>访问域名www.baidu.com——>dns服务器，返回该域名IP地址
## 2.客户端 ——> IP + port ——> 访问 网页数据。（TCP 连接。 HTTP协议。）

换言之， 对于普通的上网过程，系统其实是这样做的：浏览器本身是一个客户端，当你输入 URL 的时候，首先浏览器会去请求 DNS 服务器，通过 DNS 获取相应的域名对应的 IP，然后通过IP 地址找到 IP 对应的服务器后，要求建立 TCP 连接，等浏览器发送完 HTTP Request（请求）包后，服务器接收到请求包之后才开始处理请求包，服务器调用自身服务，返回HTTP Response（响应）包；客户端收到来自服务器的响应后开始渲染这个 Response 包里的主体（body），等收到全部的内容随后断开与该服务器之间的 TCP 连接。
 
# http和URL


## 1. URL(Uniform Resource Locator)是“统一资源定位符”(统一资源定位。 在网络环境中唯一定位一个资源数据。  浏览器地址栏内容。)的英文缩写，用于描述一个网络上的资源, 基本格式如下：
        
    schema://host[:port#]/path/.../[?query-string][#anchor]

    scheme 指定低层使用的协议(例如：http, https, ftp)

    host HTTP 服务器的 IP 地址或者域名

    port# HTTP 服务器的默认端口是 80，这种情况下端口号可以省略。如果使用了别的端口，必须指明，例如 http://www.cnblogs.com:8080/

    path 访问资源的路径

    query-string 发送给 http 服务器的数据
    anchor 锚

## 2. DNS(Domain Name System)是“域名系统”的英文缩写，是一种组织成域层次结构的计算机和网络服务命名系统，它用于 TCP/IP 网络，它从事将主机名或域名转换为实际 IP 地址的工作。DNS 就是这样的一位“翻译官”。

## 3.http 超文本传输协议。规定了 浏览器访问 Web服务器 进行数据通信的规则。  http（明文） -- TLS、SSL -- https（加密）

# http请求包

## 1. 我们先来看看 Request 包的结构, Request 包分为 3 部分，第一部分叫 Request line（请求行）, 第二部分叫 Request header（请求头）,第三部分是 body（主体）。header 和 body之间有个空行，请求包的例子所示:

    GET /domains/example/ HTTP/1.1 //请求行: 请求方法 请求 URI HTTP 协议/协议版本

    Host：www.iana.org //服务端的主机名

    User-Agent：Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.4 (KHTML, like Gecko)

    Chrome/22.0.1229.94 Safari/537.4 //浏览器信息

    Accept：text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8 //客户端能接收的

    mine

    Accept-Encoding：gzip,deflate,sdch //是否支持流压缩

    Accept-Charset：UTF-8,*;q=0.5 //客户端字符编码集

    //空行,用于分割请求头和消息体
    //消息体,请求资源参数,例如 POST 传递的参数

# http应答包
## 1. HTTP 的 response 包，他的结构如下：
   
     HTTP/1.1 200 OK //状态行
    Server: nginx/1.0.8 //服务器使用的 WEB 软件名及版本
   
     Date:Date: Tue, 30 Oct 2012 04:14:25 GMT //发送时间
    
    Content-Type: text/html //服务器发送信息的类型
    
    Transfer-Encoding: chunked //表示发送 HTTP 包是分段发的
   
     Connection: keep-alive //保持连接状态
    
    Content-Length: 90 //主体内容长度
    
    //空行 用来分割消息头和主体
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"... //消息体

## 2.Response 包中的第一行叫做状态行，由 HTTP 协议版本号， 状态码， 状态消息 三部分组成。状态码用来告诉 HTTP 客户端,HTTP 服务器是否产生了预期的 Response。HTTP/1.1 协议中定义了 5 类状态码， 状态码由三位数字组成，第一个数字定义了响应的类别。
    • 1XX 提示信息 - 表示请求已被成功接收，继续处理

    • 2XX 成功 - 表示请求已被成功接收，理解，接受

    • 3XX 重定向 - 要完成请求必须进行更进一步的处理

    • 4XX 客户端错误 - 请求有语法错误或请求无法实现

    • 5XX 服务器端错误 - 服务器未能实现合法的请求
我们看下面这个图展示了详细的返回信息，左边可以看到有很多的资源返回码，200 是常用的，表示正常信息，302 表示跳转。response header 里面展示了详细的信息。

# http WEB服务器
## 1.注册回调函数
```go
http.HandleFunc("/", handler)

func handler(w http.ResponseWriter, r *http.Request)      //w：给客户端回发数据   r：从客户端读到的数据

type ResponseWriter interface {
    Header() Header
    Write([]byte) (int, error)
    WriteHeader(int)
}

type Request struct {
    Method string      // 浏览器请求方法 GET、POST…
    Header Header    // 请求头部
    Body io.ReadCloser   // 请求包体
    GetBody func() (io.ReadCloser, error)
    ContentLength int64
    TransferEncoding []string
    // Close indicates whether to close the connection after
    Close bool
    Host string
    Form url.Values
    PostForm url.Values
    MultipartForm *multipart.Form
    Trailer Header
    RemoteAddr string    // 浏览器地址
    RequestURI string      // 浏览器请求的访问路径
    TLS *tls.ConnectionState
    Cancel <-chan struct{}
    Response *Response
    ctx context.Context
}
```
## 2. 绑定服务器地址结构：http.ListenAndServe("127.0.0.1:8009", nil)

    ——参2：通常传 ni 。 表示  让服务端 调用默认的 http.DefaultServeMux 进行处理
## 代码实现：
```go
package main

import (
    "net/http"
    "fmt"
    "os"
)

func handler(w http.ResponseWriter, r *http.Request) {
    buf := make([]byte, 4096)
    f, err := os.Open("D:/go" + r.URL.String())
    if err != nil {
        fmt.Println("os.Open err ", err)
        return
    }
    defer f.Close()
    for {
        n, err := f.Read(buf)
        if n == 0 {
            fmt.Println("加载完毕！")
            return
        }
        if err != nil {
            fmt.Println("f.Read err ", err)
            return
        }
        w.Write(buf[:n])
    }

    fmt.Println("Header: ", r.Header)
    fmt.Println("Method: ", r.Method)
    fmt.Println("URL ", r.URL)
    fmt.Println("Body ", r.Body)
    fmt.Println("Proto ", r.Proto)
    fmt.Println("RemoteAddr ", r.RemoteAddr)
}
func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe("192.168.15.53:8009", nil )     // 让服务端 调用默认的 http.DefaultServeMux 进行处理
}
```

# http WEB客户端
## ​1.获取web服务器数据
## 2. defer resp.Body.Close()
## 3. for 循环提取 Body 数据：
```go
for {
    n, err := resp.Body.Read(buf )
    if n == 0 {
        fmt.Println("re!ad finish!")
        break
    }
    if err != nil && err == io.EOF {
        fmt.Println("resp.Body.Read err ", err)
        break
    }
}
```
## 代码实现
```go
package main

import (
    "net/http"
    "fmt"
    "io"
)

func main() {
    resp, err := http.Get("http://127.0.0.1:8009/cls.jpg")
    if err != nil {
        fmt.Println("http.Get err ", err)
        return
    }
    defer resp.Body.Close()
    fmt.Println("Header ", resp.Header)
    fmt.Println("Status ", resp.Status)
    fmt.Println("StatusCode ", resp.StatusCode)
    fmt.Println("Proto ", resp.Proto)

    buf := make([]byte, 4096)
    var result string
    for {
        n, err := resp.Body.Read(buf)
        if n == 0 {
            fmt.Println("re!ad finish!")
            break
        }
        if err != nil && err == io.EOF {
            fmt.Println("resp.Body.Read err ", err)
            break
        }
        result += string(buf[:n])
    }
    fmt.Println(result)
}
```