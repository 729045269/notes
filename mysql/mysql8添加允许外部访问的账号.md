# mysql8添加允许外部访问的账号

```
1.允许所有ip访问：

CREATE USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '密码';

GRANT ALL PRIVILEGES ON *.* TO 'root' @'%' WITH GRANT OPTION;

2.允许指定ip访问：

CREATE USER 'root'@'192.168.0.10' IDENTIFIED WITH mysql_native_password BY '123456';

GRANT ALL PRIVILEGES ON *.* TO 'root' @'192.168.0.10' WITH GRANT OPTION;

3.指定ip段访问
CREATE USER 'root' @'192.%' IDENTIFIED WITH mysql_native_password BY '123456';

GRANT ALL PRIVILEGES ON *.* TO 'root' @'192.%' WITH GRANT OPTION;
#指定数据库
GRANT ALL PRIVILEGES ON testdb.* TO 'root' @'192.%' WITH GRANT OPTION;


3.刷新权限：

flush privileges;

4.删除用户

drop user '用户名'@'主机名';
drop user 'root'@'192.168.0.10';
```

