# Twemproxy 的使用

1.编译安装：



autoconf下载地址：[http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz](https://link.zhihu.com/?target=http%3A//ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz)

twemproxy下载地址：[https://codeload.github.com/twitter/twemproxy/zip/master](https://link.zhihu.com/?target=https%3A//codeload.github.com/twitter/twemproxy/zip/master)

twemproxy的安装要求autoconf的版本在2.64以上，否则提示"error: Autoconf version 2.64 or higher is required"。



查找旧版本autoconf，并且卸载

```text
rpm -qf /usr/bin/autoconf  #版本号如果在2.64以上，则不用执行下面的命令
rpm -e --nodeps autoconf-2.63   
```

安装最新版本

```text
tar zxvf autoconf-2.69.tar.gz 
cd autoconf-2.69 
./configure --prefix=/usr 
make && make install 

编译安装twemproxy

unzip twemproxy-master.zip
cd twemproxy-master
autoreconf -fvi
./configure --prefix=/usr/local/twemproxy
make -j 8
make install
设置环境变量：

 echo "PATH=$PATH:/usr/local/twemproxy/sbin/" >> /etc/profile
 source /etc/profile
2.创建相关目录（存放配置文件和pid文件）

cd /usr/local/twemproxy
mkdir run conf
3.添加proxy配置文件

vim /usr/local/twemproxy/conf/nutcracker.yml
内容如下（配置文件参考官方），
***下面这段配置我是一行一行复制过去的，整段复制过去会有问题，不知道是空格导致的还是什么导致的，暂时找不到原因

alpha:
  listen: 127.0.0.1:22121 #如果需要外部可以访问就设置成 0.0.0.0:22121
  hash: fnv1a_64
  distribution: ketama
  auto_eject_hosts: true
  redis: true
  #redis_auth: 123456 #redis密码
  server_connections: 10 #每个服务器可以打开的最大连接数。默认情况下，我们最多打开 1 个服务器连接
  server_retry_timeout: 2000
  server_failure_limit: 1
  servers:
   - 127.0.0.1:6379:1
   - 127.0.0.1:7001:1
   - 127.0.0.1:7002:1


4.启动Twemproxy服务

[root@redis-server ~]# nutcracker -t /usr/local/twemproxy/conf/nutcracker.yml
nutcracker: configuration file 'conf/nutcracker.yml' syntax is ok

启动命令：

nutcracker -d -c /usr/local/twemproxy/conf/nutcracker.yml -p /usr/local/twemproxy/run/redisproxy.pid -o /usr/local/twemproxy/run/redisproxy.log

杀掉进程：
ps -ef|grep nutcracker|awk '{print $2}'|xargs kill -9


nutcracker用法与命令选项

Usage: nutcracker [-?hVdDt] [-v verbosity level] [-o output file]
                  [-c conf file] [-s stats port] [-a stats addr]
                  [-i stats interval] [-p pid file] [-m mbuf size]

Options:
-h, –help                        : 查看帮助文档，显示命令选项
-V, –version                     : 查看nutcracker版本
-t, –test-conf                   : 测试配置脚本的正确性
-d, –daemonize                   : 以守护进程运行
-D, –describe-stats              : 打印状态描述
-v, –verbosity=N                 : 设置日志级别 (default: 5, min: 0, max: 11)
-o, –output=S                    : 设置日志输出路径，默认为标准错误输出 (default: stderr)
-c, –conf-file=S                 : 指定配置文件路径 (default: conf/nutcracker.yml)
-s, –stats-port=N                : 设置状态监控端口，默认22222 (default: 22222)
-a, –stats-addr=S                : 设置状态监控IP，默认0.0.0.0 (default: 0.0.0.0)
-i, –stats-interval=N            : 设置状态聚合间隔 (default: 30000 msec)
-p, –pid-file=S                  : 指定进程pid文件路径，默认关闭 (default: off)
-m, –mbuf-size=N                 : 设置mbuf块大小，以bytes单位 (default: 16384 bytes)

查看进程，确认启动。

[root@redis-server run]# ps -ef | grep nutcracker | grep -v grep
root       809     1  0 03:09 ?        00:00:00 nutcracker -d -c /usr/local/twemproxy/conf/nutcracker.yml -p /usr/local/twemproxy/run/redisproxy.pid -o /usr/local/twemproxy/run/redisproxy.log


5. 简单测试

[root@redis-server ~]# netstat -nltp | grep nutcracker
tcp        0      0 0.0.0.0:22222               0.0.0.0:*                   LISTEN      809/nutcracker      
tcp        0      0 127.0.0.1:22121             0.0.0.0:*                   LISTEN      809/nutcracker      
[root@redis-server ~]# redis-cli -p 22121             
127.0.0.1:22121> set name test
OK
127.0.0.1:22121> get name
"test"
127.0.0.1:22121> 


 

参考资料：

https://github.com/twitter/twemproxy
```