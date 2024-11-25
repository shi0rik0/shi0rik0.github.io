---
title: "Python标准库的简易HTTP服务器"
date: 2024-06-30T08:45:07+08:00
draft: false
---

Python 标准库有一个名为`http.server`的模块，提供了简易的 HTTP 服务器功能。下面的示例代码用这个模块启动了一个简单的 HTTP 服务器。这个服务器只支持 POST 方法，会打印 POST 请求的信息，并返回一个固定的字符串。

```python
from http.server import BaseHTTPRequestHandler, HTTPServer


class MyHandler(BaseHTTPRequestHandler):

    def do_POST(self):
        print('method:', self.command)
        print('path:', self.path)
        print('HTTP version:', self.request_version)
        print('headers:', self.headers)
        content_length = int(self.headers['Content-Length'])
        body = self.rfile.read(content_length).decode()
        print('body:', body)

        response = b'Roger!'
        self.send_response(200)
        self.send_header('Content-Length', len(response))
        self.end_headers()
        self.wfile.write(response)


if __name__ == '__main__':
    server = HTTPServer(('0.0.0.0', 8080), MyHandler)
    server.serve_forever()
```

要测试这个服务器，可以发送一个简单的 POST 请求：

```python
import requests

res = requests.post('http://localhost:8080/', data='Hello!')
print(res.text)
```

在服务端应该会看到类似下面的输出：

```
method: POST
path: /
HTTP version: HTTP/1.1
headers: Host: localhost:8080
User-Agent: python-requests/2.32.3
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
Content-Length: 6


body: Hello!
```

而客户端则会收到如下的响应：

```
Roger!
```

下面对示例代码的关键部分进行更加详细地说明。

HTTP 服务器的具体逻辑是通过继承`BaseHTTPRequestHandler`基类来实现的。当 HTTP 服务器收到 GET 请求时，就会调用对象的`do_GET()`方法，收到 POST 请求时会调用对象的`do_POST()`方法，以此类推。

`BaseHTTPRequestHandler`基类提供了访问 HTTP 请求内容以及发送响应的接口。

HTTP 请求的 request line 可以通过`command`、`path`和`request_version`属性来获取，它们都是字符串。请求的头部信息可以通过`headers`属性获取。`headers`的类型是`http.client.HTTPMessage`，是一个类似于`dict`的对象。通过读取`rfile`，可以读取请求的 body 部分。

发送响应只需要往`wfile`中写入数据即可。发送 HTTP 响应的响应码和头部的时候，用`send_response()`和`send_header()`方法会更加方便，这些方法的底层实现依然是往`wfile`中写入数据，真正写入数据是在调用`end_headers()`的时候。

`http.server`模块本身也提供了一个已经实现了的子类`SimpleHTTPRequestHandler`。通过在命令行输入`python -m http.server 8080`就可以快速启动一个使用了`SimpleHTTPRequestHandler`的 HTTP 服务器。
