# nginx 记录post请求参数

1.nginx.conf配置

```
http{

  ...

  # escape=json 是不将参数转换成十六进制（nginx 1.11.8 以上版本支持）

  log_format access escape=json '$remote_addr - $http_x_forwarded_for  $remote_user [$time_local] "$request" $status $body_bytes_sent $request_body "$http_referer" "$http_user_agent"';

}
```

2.站点配置，api.conf:

```
server

{ 

  listen 80;

  server_name www.api.com;

  ...

  access_log /home/wwwlogs/api.log access;#在这行最后加上access

}
```

