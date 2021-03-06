---
name: Nginx 部署
sort: 3
---

# nginx 部署

Go 是一个独立的 HTTP 服务器，但是我们有些时候为了 nginx 可以帮我做很多工作，例如访问日志，cc 攻击，静态服务等，nginx 已经做的很成熟了，Go 只要专注于业务逻辑和功能就好，所以通过 nginx 配置代理就可以实现多应用同时部署，如下就是典型的两个应用共享 80 端口，通过不同的域名访问，反向代理到不同的应用。

```
server {
    listen       80;
    server_name  www.a.com;
    charset utf-8;
    access_log  /home/a.com.access.log  main;
    location / {
        proxy_pass http://127.0.0.1:8080;
    }
}

 server {
    listen       80;
    server_name  www.b.com;
    charset utf-8;
    access_log  /home/b.com.access.log  main;
    location / {
        proxy_pass http://127.0.0.1:8081;
    }
}
```