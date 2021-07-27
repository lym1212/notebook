## 引入

```
var http = require('http')
```

## 创建服务器

``` 
var server = http.createServer()    // 返回 http.server 实例
```

## 接收请求

```
// 客户端请求时，就会触发 request 事件
server.on('request', callback)
```

- 回调函数参数：
  - request ( http.IncomingMessage 类)
    - url：请求的路径
  - response ( http.ResponseServer 类)，响应数据只能是 string 或 Buffer (`JSON.stringify()`)
    - write(data)： 给客户端发送响应数据，必须用`end()`结束响应
    - end(data)： 如果指定了data，也可以发送数据

## 启动服务器

```
server.listen(8080, callback)
```

## 重定向

```javascript
res.statusCode = 302
res.setHeader('Location', '/')   // 重定向
res.end()
```

