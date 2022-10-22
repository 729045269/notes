### nginx配置syslog转发

[这里参考 Linux服务器和Nginx配置syslog转发]: https://www.cnblogs.com/yangjisen/p/12777951.html

**将Linux系统日志通过Rsyslog输出到syslog服务器**

Rsyslog是linux系统下高速的日志收集处理服务，它具有高性能、安全可靠和模块化设计的特点，能够接收各种来源的日志输入（例如：file，tcp，udp，uxsock等），并通过处理后将结果输出的不同的目的地（例如：mysql，mongodb，[elasticsearch](https://baike.baidu.com/item/elasticsearch/3411206)，[kafka](https://baike.baidu.com/item/Kafka/17930165)，日志审计服务器等），每秒处理日志量能够超过百万条。

Rsyslog作为syslog的增强升级版本已经在各linux发行版默认安装了，无需额外安装。如果操作系统中确实没有Rsyslog，可以通过yum进行安装或升级。

```
查看是否有安装rsyslog
# service rsyslog status

安装rsyslog
# yum install rsyslog
```

 