# nginx常用命令

```
常规命令

1.启动

  /usr/local/nginx/sbin/nginx -s start

2.停止

  /usr/local/nginx/sbin/nginx -s stop

3.验证配置文件是否正确

  /usr/local/nginx/sbin/nginx -t

4.重载配置文件

  /usr/local/nginx/sbin/nginx -s reload

5.重启

  /usr/local/nginx/sbin/nginx -s restart

注册了系统服务的命令

1.启动

  service nginx start / systemctl start nginx

2.停止

  service nginx stop / systemctl stop nginx

4.重载配置文件

  service nginx reload / systemctl reload nginx

5.重启

  service nginx restart / systemctl restart nginx
```

