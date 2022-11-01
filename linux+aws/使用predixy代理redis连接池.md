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

   

**修改配置文件**

​	修改 ` /usr/local/predixy/conf ` 目录下cluster.conf 文件如下:

````
ClusterServerPool {
    MasterReadPriority 60
    StaticSlaveReadPriority 50
    DynamicSlaveReadPriority 50
    RefreshInterval 1
    ServerTimeout 1
    ServerFailureLimit 10
    ServerRetryTimeout 1
    KeepAlive 120
    Servers {
        + 127.0.0.1:7001
        + 127.0.0.1:7002
    }
}
```

​	修改 `/usr/local/predixy/conf` 目录下 `predixy.conf`文件，默认predixy代理连接端口号7617

```
 Bind 0.0.0.0:7617 #将这段代码签名的注释解除
 
Include cluster.conf #把这个的注释解除
# Include sentinel.conf
#Include try.con #将这个注释
```

 	如果需要添加密码，则修改 `/usr/local/`predixy/conf 目录下auth.conf 文件，predixy代理连接密码为 #123456789#

```
Authority {
    Auth {
        Mode write
    }
    Auth "#123456789#" {
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
auth.conf，访问权限控制配置，可以定义多个验证密码，可每个密码指定读、写、管理权限，以及定义可访问的健空间
dc.conf，多数据中心支持，可以定义读写分离规则，读流量权重分配
latency.conf， 延迟监控规则定义，可以指定需要监控的命令以及延时时间间隔
```



#### auth.conf

```bash
## 密码直接配置成redis的实例的一样或者不一样都行，就是访问predixy代理的密码。
Authority {
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

#### predixy.conf

```php
# 一些端口什么的随自己配置了，主要配置下执行哪几个子conf文件，禁用掉 sentinel和try就好了，sentunel和cluster只能二选一，try就是测试的。
Include auth.conf
Include cluster.conf
# Include sentinel.conf
# Include try.conf
```

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


-----------------------------------
