# mysql8添加允许外部访问的账号

```
1.允许所有ip访问：

CREATE USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '密码';

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';

2.允许指定ip访问：

CREATE USER 'root'@'192.168.0.10' IDENTIFIED WITH mysql_native_password BY '123456';

GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.0.10';

3.刷新权限：

flush privileges;
```

