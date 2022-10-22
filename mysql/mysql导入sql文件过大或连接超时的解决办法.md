# mysql导入sql文件过大或连接超时的解决办法

```
# 执行以下命令
SET GLOBAL max_allowed_packet=1073741824;
SET GLOBAL net_buffer_length=1048576;
SET GLOBAL interactive_timeout=28800000;
SET GLOBAL wait_timeout=28800000

max_allowed_packet=XXX 客户端/服务器之间通信的缓存区的最大大小;
net_buffer_length=XXX TCP/IP和套接字通信缓冲区大小,创建长度达net_buffer_length的行
interactive_timeout = 10; 对后续起的交互链接有效；
wait_timeout 对当前交互链接有效；
```

