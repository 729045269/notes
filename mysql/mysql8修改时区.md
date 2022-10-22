### mysql8修改时区

```
在my.cnf加上下面配置，然后重启mysql
[mysqld]
default-time_zone = '+8:00'
```

查看时区

```
show variables like'%time_zone';
```

