# 使用predixy 连接 redis

1. 安装依赖包：` yum install libstdc++-static -y  `

2. 下载文件：` wget https://github.com/joyieldInc/predixy/archive/1.0.5.tar.gz`

3.  解压 ：`tar -zxvf 1.0.5.tar.gz `

4. 进入解压目录： ` cd predixy-1.0.5`

5.  编译predixy:  `make -j4`

6. 在`/usr/local` 下面创建 predixy  目录： ` mkdir /usr/local/predixy `

7. 将编译好的predixy和conf文件夹移动到`/usr/local/predixy` : 

   ````
   cp src/predixy /usr/local/predixy
   cp -r conf /usr/local/predixy
   ````

   

**修改配置文件**（集群）

​	修改 ` /usr/local/predixy/conf ` 目录下cluster.conf 文件如下:

````
	修改 `/usr/local/predixy/conf` 目录下 `predixy.conf`文件，默认predixy代理连接端口号7617

```
Bind 0.0.0.0:7617 #将这段代码签名的注释解除
 
Include cluster.conf #把这个的注释解除
# Include sentinel.conf
#Include try.conf #将这个注释
```

 	如果需要添加密码，则修改 /usr/local/predixy/conf 目录下auth.conf 文件，predixy代理连接密码为 #123456789#

```
Authority {
#    Auth {  #这行直接删除
#        Mode write #这行直接删除
#    }	#这行直接删除
    Auth "#123456789#" {
	    #admin有读写权限
        Mode admin
    }
}
```



**启动predixy**

首先进入目录：cd /usr/local/predixy 
启动： nohup predixy conf/predixy.conf > /tmp/predixy.log 2>&1 &；



# 配置文件说明:



```css
predixy.conf，整体配置文件，会引用下面的配置文件
cluster.conf，用于Redis Cluster时，配置后端redis信息
sentinel.conf，用于Redis Sentinel时，配置后端redis信息
standalone.conf 单机模式
auth.conf，访问权限控制配置，可以定义多个验证密码，可每个密码指定读、写、管理权限，以及定义可访问的健空间
dc.conf，多数据中心支持，可以定义读写分离规则，读流量权重分配
latency.conf， 延迟监控规则定义，可以指定需要监控的命令以及延时时间间隔
```



#### auth.conf

```bash
## 密码直接配置成redis的实例的一样或者不一样都行，就是访问predixy代理的密码。
Authority {
#    Auth {  #这行直接删除
#        Mode write #这行直接删除
#    }	#这行直接删除

    Auth "123456" {
        Mode admin
    }
}
```



#### cluster.conf

```bash
ClusterServerPool {
    MasterReadPriority 100  #这个是主节点访问权重，如果是只把备节点用作备份不去做读写分离，直接将这个配置成100只去读主节点就好了。
    Password sjwkk123456 # redis实例的访问密码
    StaticSlaveReadPriority 0  # 读写分离功能，从静态redis slave节点执行读请求的优先级，所谓静态节点，是指在本配置文件中显示列出的redis节点，不指定的话为0
    DynamicSlaveReadPriority 0 # 功能见上，所谓动态节点是指在本配置文件中没有列出，但是通过redis sentinel动态发现的节点，不指定的话为0
    RefreshInterval 1 # predixy会周期性的请求redis sentinel以获取最新的集群信息，该参数以秒为单位指定刷新周期，不指定的话为1秒
    ServerTimeout 1 # 请求在predixy中最长的处理/等待时间，如果超过该时间redis还没有响应的话，那么predixy会关闭同redis的连接，并给客户端一个错误响应，对于blpop这种阻塞式命令，该选项不起作用，为0则禁止此功能，即如果redis不返回就一直等待，不指定的话为0
    ServerFailureLimit 10 # 一个redis实例出现多少次才错误以后将其标记为失效，不指定的话为10
    ServerRetryTimeout 1 # 一个redis实例失效后多久后去检查其是否恢复正常，不指定的话为1秒
    KeepAlive 120 #predixy与redis的连接tcp keepalive时间，为0则禁止此功能，不指定的话为0
    Servers {
    ##配置所有节点地址就好了
        + 127.0.0.1:7001
        + 127.0.0.1:7002
        + 127.0.0.1:7003
        + 127.0.0.1:7004
        + 127.0.0.1:7005
        + 127.0.0.1:7006
    }
}
```

### standalone.conf 单机模式
StandaloneServerPool {
    Password password    # Redis 服务器的认证密码
    Databases 1          # Redis 服务器的数据库数量
    Hash crc16           # 指定哈希函数使用的算法，可选的值为 crc16、atol、fnv1_64
    HashTag "{}"         # 指定哈希标签，可以用于将多个键映射到同一个槽位
    Distribution modula  # 指定哈希槽的分配方式，可选的值为 modula、random
    MasterReadPriority 50     # 指定主节点的读取优先级，范围为 0~100
    StaticSlaveReadPriority 0 # 指定静态从节点的读取优先级，范围为 0~100
    DynamicSlaveReadPriority 0 # 指定动态从节点的读取优先级，范围为 0~100
    RefreshMethod sentinel # 指定集群的监控方式，可选的值为 fixed、sentinel
    RefreshInterval 1         # 指定定时刷新哈希槽的时间间隔，单位为秒
    ServerTimeout 0           # 指定连接 Redis 服务器的超时时间，单位为秒
    ServerFailureLimit 10     # 指定服务器连接失败的次数上限
    ServerRetryTimeout 1      # 指定重新连接服务器的超时时间，单位为秒
    KeepAlive 0               # 指定服务器连接的 TCP KeepAlive 时间，单位为秒
    Servers {
        + host:port [password] # 指定 Redis 服务器的地址和端口号，可以选择性地指定认证密码
    }
    Group group1 {
        Master {
            host:port [password] # 指定主节点的地址和端口号，可以选择性地指定认证密码
        }
        StaticSlaves {
            host:port [password] # 指定静态从节点的地址和端口号，可以选择性地指定认证密码
            ...
        }
        DynamicSlaves {
            host:port [password] # 指定动态从节点的地址和端口号，可以选择性地指定认证密码
            ...
        }
    }
    ...
}

#### 下面是一个单机redis的配置，standalone.conf：
StandaloneServerPool {
    Password 123456
    Databases 16
    Hash crc16
#    HashTag "{}"
    Distribution modula
#    MasterReadPriority 60
#    StaticSlaveReadPriority 50
#    DynamicSlaveReadPriority 50
    RefreshMethod fixed
#    RefreshInterval 1
    ServerTimeout 0
    ServerFailureLimit 3
    ServerRetryTimeout 1
    KeepAlive 0
    Group master1 {
        + 192.168.0.1:6379
    }
}

这里的配置中：
Password 表示 Redis 的密码，如果没有密码可以省略
Databases 表示 Redis 的数据库数量，默认是 1
Hash 表示使用的哈希算法，可以选择 atol 或 crc16
Distribution 表示数据分布的策略，可以选择 modula 或 random
ServerTimeout 表示 Redis 服务端连接的超时时间，默认是 0，表示无限等待
ServerFailureLimit 表示 Redis 服务端连接失败的次数，默认是 10
ServerRetryTimeout 表示 Redis 服务端连接失败后的重试时间间隔，默认是 1 秒
KeepAlive 表示 Redis 服务端连接的 TCP keepalive 时间，默认是 0，表示不启用
在 Group 中配置 Redis 的地址和端口，这里只配置了一个单机的 Redis 服务，如果有多个可以添加多个。


#### predixy.conf 配置看下面


#### 启动predixy

```jsx
nohup predixy conf/predixy.conf  >/dev/null 2>&1 &
```


````



下面的报告参考地址：`https://blog.51cto.com/u_15127663/4187151`

### predixy代理测试

在代理 WorkerThreads 3 时最佳；

在常规测试下 workerThreads 2-4区别不大

它的多线程指的是，predixy 发起多个线程，去连接集群节点；感觉和多个client连接没什么区别；

举例：

假如外部连接到代理，100个连接；代理开了4线程；

那么有50个连接的请求被分配到A节点，前面说了**代理的4线程就相当于代理弄了4个连接**连到集群节点；

所以这里就4个连接来承接50个连接的请求，最终整理好后由这4个连接来对集群节点发起请求操作



workerThreads 设置好之后，最后使用redis-benchmark来测试响应时间，来调整workerThreads 的值

```
命令： ./redis-benchmark -h 127.0.0.1 -p 7617 -a 123456 -c 20 -n 100000 -t set,get -q
结果：
SET: 11971.75 requests per second, p50=0.847 msec                   
GET: 12029.35 requests per second, p50=0.823 msec   
GET表示执行的Redis命令类型是GET，12029.35 requests per second表示平均每秒执行的请求数是12029.35，也就是Redis处理请求的能力，这个数值越大表示Redis的性能越好；p50=0.823 msec表示50%的请求响应时间小于等于0.823毫秒，也就是Redis的响应速度，这个数值越小表示Redis的响应速度越快。
```






-----------------------------------



predixy.conf配置说明：

```
################################### GENERAL ####################################
## Predixy configuration file example

## Specify a name for this predixy service
## redis command INFO can get this
Name PredixyExample

## Specify listen address, support IPV4, IPV6, Unix socket
## Examples:
# Bind 127.0.0.1:7617
# Bind 0.0.0.0:7617
# Bind /tmp/predixy

## Default is 0.0.0.0:7617
Bind 0.0.0.0:7617 #监听端口

## Worker threads
# WorkerThreads 配置项表示工作线程的数量，这是一个非常重要的配置项。WorkerThreads 数量越多，可以处理的并发连接数量就越多，但同时也会增加 CPU 的负担。在配置 WorkerThreads 数量时，需要考虑到服务器的硬件性能以及客户端并发连接数的情况
WorkerThreads 1 # 建议将 WorkerThreads 参数的值设置为 CPU 核心数的两倍，以获得更好的并发能力和响应速度。

## Memory limit, 0 means unlimited

## Examples:
# MaxMemory 100M
# MaxMemory 1G
# MaxMemory 0

## MaxMemory can change online by CONFIG SET MaxMemory xxx
## Default is 0
# MaxMemory 0

## Close the connection after a client is idle for N seconds (0 to disable)
## ClientTimeout can change online by CONFIG SET ClientTimeout N
## Default is 0
ClientTimeout 300


## IO buffer size
## Default is 4096
# BufSize 4096

################################### LOG ########################################
## Log file path
## Unspecify will log to stdout
## Default is Unspecified
# Log ./predixy.log

## LogRotate support

## 1d rotate log every day
## nh rotate log every n hours   1 <= n <= 24
## nm rotate log every n minutes 1 <= n <= 1440
## nG rotate log evenry nG bytes
## nM rotate log evenry nM bytes
## time rotate and size rotate can combine eg 1h 2G, means 1h or 2G roate a time

## Examples:
# LogRotate 1d 2G
# LogRotate 1d

## Default is disable LogRotate


## In multi-threads, worker thread log need lock,
## AllowMissLog can reduce lock time for improve performance
## AllowMissLog can change online by CONFIG SET AllowMissLog true|false
## Default is true
# AllowMissLog false

## LogLevelSample, output a log every N
## all level sample can change online by CONFIG SET LogXXXSample N
LogVerbSample 0
LogDebugSample 0
LogInfoSample 10000
LogNoticeSample 1
LogWarnSample 1
LogErrorSample 1


################################### AUTHORITY ##################################
Include auth.conf

################################### SERVERS ####################################
# 一些端口什么的随自己配置了，主要配置下执行哪几个子conf文件，禁用掉 sentinel和try就好了，sentunel和cluster只能二选一，try就是测试的。如果需要单机模式，则要禁用掉sentunel和cluster，开启standalone.conf
# Include cluster.conf #集群模式
# Include sentinel.conf #哨兵模式
# Include try.conf #测试的
Include standalone.conf #单机redis


################################### DATACENTER #################################
## LocalDC specify current machine dc
# LocalDC bj

## see dc.conf
# Include dc.conf


################################### COMMAND ####################################
## Custom command define, see command.conf
#Include command.conf

################################### LATENCY ####################################
## Latency monitor define, see latency.conf
Include latency.conf
```



### 开机自启：


要让CentOS在开机时自动执行指定的脚本，可以通过在系统启动时执行该脚本的方法来实现。你可以使用systemd服务来管理该过程。下面是在CentOS上配置systemd服务的步骤：

1. 创建一个新的systemd服务单元： 使用文本编辑器（如`vi`或`nano`）创建一个新的service文件，比如`predixy.service`：

   ```
   sudo nano /etc/systemd/system/predixy.service
   ```
   
2. 将以下内容粘贴到`predixy.service`文件中：

   ```
   Description=Predixy Service
   After=network.target
   
   [Service]
   Type=simple
   ExecStart=/usr/local/predixy/predixy /usr/local/predixy/conf/predixy.conf
   
   [Install]
   WantedBy=multi-user.target
   ```
   
   这个配置指定了服务的描述、启动时依赖的目标（network.target，表示在网络服务启动后再启动该服务）、服务的类型（simple表示脚本不会派生子进程）和执行的命令。
   
3. 保存并关闭文件（在`nano`中按`Ctrl + X`，然后按`Y`保存）。

4. 启用该服务： 运行以下命令启用服务并使其在开机时自动执行：

   ```
   sudo systemctl enable predixy
   ```
   
5. 启动服务： 运行以下命令来启动该服务：

   ```
   sudo systemctl start predixy
   ```
   
   现在，Predixy服务应该已经在系统启动时自动执行了。
   
6. 验证服务状态（可选）： 如果想确认服务是否正在运行，可以运行以下命令查看服务状态：

   ```
   sudo systemctl status predixy
   ```
   
如果一切顺利，你应该能够看到服务正在运行的状态信息。

现在，当你重启CentOS系统时，Predixy服务应该会自动启动并执行你指定的脚本
