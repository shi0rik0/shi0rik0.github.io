---
title: "Nginx入门教程"
date: 2024-09-05T17:19:00+08:00
draft: false
---

## 启动、终止与重新加载

启动nginx：
```
nginx -c conf/nginx.conf
```

`-c`参数可以指定配置文件的路径，默认为`conf/nginx.conf`。

终止nginx：
```
nginx -s quit
```

重新加载配置文件：
```
nginx -s reload
```

## 最简单的配置文件

下面的配置文件会监听8000端口，并将网站根目录设为`/www/data`。

```
events {

}

http {
    server {
        listen 8000;

        location / {
            root /www/data;
        }
    }
}
```

这里要补充几点，首先是http块下面可以有多个server块，例如：
```
http {
    server {
        listen 8000;

        location / {
            root /www/data;
        }
    }
    
    server {
        listen 8001;

        location / {
            root /www/data2;
        }
    }
}
```

这样就会同时监听8000和8001两个端口。

还有一点就是`location /`中的`/`是用来匹配网站的根目录，这里也就是匹配了网站的所有路径。一个server块下面也可以有多个location块，用来匹配不同的路径。比如说：
```
http {
    server {
        listen 8000;

        location / {
            root /www/data;
        }

        location /images/ {
            root /www/data2;
        }
    }
}
```

这样一来，访问`http://localhost:8000/images/img.png`的时候就会返回`/www/data2/images/img.png`。


## 反向代理

```
http {
    server {
        listen 8000;

        location / {
            proxy_pass http://127.0.0.1:8001;
            proxy_set_header Host $host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
```

上面就是一个最基本的反向代理配置，会将8000端口收到的消息转发到本地的8001端口。两行`proxy_set_header`配置是可选的，它们会在转发消息的头部增加`Host`和`X-Forwarded-For`两个字段，这些字段可能会被一些上游应用使用。

## 负载均衡

```
http {
    upstream backend {
        server 127.0.0.1:8001 weight=3;
        server 127.0.0.1:8002;
    }

    server {
        listen 8000;

        location / {
            proxy_pass http://backend;
            proxy_set_header Host $host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
```

上面就是一个最基本的负载均衡配置。Nginx默认采用的是Round Robin算法，每个服务器默认权重为1。上面的配置文件下，两个上游服务器的负载比约为3：1。
