> 常用状态码类别

HTTP 状态码是在 HTTP 协议中服务器向客户端返回的 3 位数字代码，用于表示请求的处理状态。常见的 HTTP 状态码有以下几类：

1. 1xx：信息性状态码，表示请求已被接受，需要继续处理
2. 2xx：成功状态码，表示请求已成功处理并返回
3. 3xx：重定向状态码，表示需要进一步操作才能完成请求
4. 4xx：客户端错误状态码，表示客户端发送的请求有误
5. 5xx：服务器错误状态码，表示服务器处理请求时出现错误



> 常见 HTTP 状态码

| 状态码 | 描述                  | 解释                                               |
| :----- | --------------------- | -------------------------------------------------- |
| 100    | Continue              | 客户端应继续其请求                                 |
| 101    | Switching Protocols   | 服务器根据客户端请求切换协议，                     |
| 200    | OK                    | 请求成功，服务器成功处理了请求                     |
| 201    | Created               | 请求已被成功处理，并且在服务器上创造了新的资源     |
| 204    | No Content            | 服务器成功处理请求，但没有返回任何内容             |
| 301    | Moved Permanly        | 请求的资源已永久移动到新位置                       |
| 302    | Found                 | 请求的资源临时从不同 URL 响应请求                  |
| 304    | Not Modified          | 资源未修改，客户端可以使用缓存版本                 |
| 400    | Bad Request           | 请求有语法错误或无法被服务器理解                   |
| 401    | Unauthorized          | 请求需要用户身份验证                               |
| 403    | Forbidden             | 服务器已理解请求，但拒绝执行                       |
| 404    | Not Found             | 请求的资源不存在                                   |
| 500    | Internal Server Error | 服务器遇到一个未曾预料的状况，导致无法完成请求处理 |
| 502    | Bad Gateway           | 服务器作为网关或代理，从上游服务器收到无效的响应   |
| 503    | Service Unavailable   | 服务器当前无法处理请求                             |





