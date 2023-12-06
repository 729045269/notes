# MySQL中间件ProxySQL2.x安装及测试

1.下载安装(Centos8)

```
$wget https://github.com/sysown/proxysql/releases/download/v2.5.5/proxysql-2.5.5-1-centos8-clang.x86_64.rpm
#安装
$rpm -ivh proxysql-2.5.5-1-centos8-clang.x86_64.rpm 
#查看版本
$proxysql --version
	ProxySQL version 2.5.5-1-g877cab1, codename Truls
```



**启停/状态**

```
#查看版本
$proxysql --version
#存储目录
#ls  /var/lib/proxysql/
#启动/停止
$service proxysql start
$service proxysql stop
$service proxysql status  # 查看proxysql状态
ProxySQL is running (20761).
```



##### 亚马逊的AWS系统服务器下载安装

使用docker安装（因为gnutls这个库是系统重要的库，与proxysql的版本不一致，不敢乱升级）

```
amazon-linux-extras install docker
#启动docker
service docker start
#开机自启
chkconfig docker on
#将当前用户添加到 docker 用户组：为了让当前用户可以运行 Docker 命令，运行以下命令将当前用户添加到 docker 用户组。请确保将 "your_username" 替换为你当前登录用户的用户名。
usermod -aG y root
#查看版本
docker --version
```

拉取 ProxySQL 镜像：运行以下命令来拉取官方的 ProxySQL Docker 镜像。

```
docker pull proxysql/proxysql
```



### 注意：您需要定义第二对管理凭据才能连接到容器外部。

1.在/usr/local/proxysql下创建`proxysql.cnf`，并配置以下内容

```
datadir="/var/lib/proxysql"
#错误日志配置
errorlog="/var/lib/proxysql/proxysql.log" 

admin_variables=
{
	admin_credentials="admin:admin;radmin:radmin"
	mysql_ifaces="0.0.0.0:6032"
}

mysql_variables=
{
	threads=4
	max_connections=2048
	default_query_delay=0
	default_query_timeout=36000000
	have_compress=true
	poll_timeout=2000
	interfaces="0.0.0.0:6033"
	default_schema="information_schema"
	stacksize=1048576
	server_version="5.5.30"
	connect_timeout_server=3000
	monitor_username="monitor"
	monitor_password="monitor"
	monitor_history=600000
	monitor_connect_interval=60000
	monitor_ping_interval=10000
	monitor_read_only_interval=1500
	monitor_read_only_timeout=500
	ping_interval_server_msec=120000
	ping_timeout_server=500
	commands_stats=true
	sessions_sort=true
	connect_retries_on_failure=10
}

```

2.启动docker

```
#将宿主机的/usr/local/proxysql.cnf和/usr/local/proxysql目录挂载到docker
$ docker run -p 16032:6032 -p 16033:6033 -p 16070:6070 -d -v /usr/local/proxysql/proxysql.cnf:/etc/proxysql.cnf  -v /usr/local/proxysql:/var/lib/proxysql proxysql/proxysql
```

3.连接proxysql的docker

```
mysql -h127.0.0.1 -P16032 -uradmin -pradmin --prompt "ProxySQL Admin>"
```



#### docker操作命令

查找容器 ID：首先，你需要查找运行的容器的 ID。运行以下命令来查找 ProxySQL 容器的 ID：

```
docker ps
```

上述命令将列出当前正在运行的 Docker 容器，找到名为 `proxysql/proxysql` 的容器，并记录其 CONTAINER ID。

停止容器：使用以下命令来停止容器。将 `CONTAINER_ID` 替换为上一步记录到的 ProxySQL 容器的实际 ID。

```
docker stop CONTAINER_ID
```

1. 等待容器停止：等待一段时间，直到容器完全停止。
2. 检查容器状态：运行以下命令来确认容器是否已经停止。

```
docker ps -a
```



##### 设置proxysql连接到mysql的信息

```
INSERT INTO mysql_servers(hostgroup_id, hostname, port) VALUES (1, '192.168.0.12', 3306);
```


###### 配置代码连接到proxysql用户名账户信息

```
INSERT INTO mysql_users(username, password, default_hostgroup) VALUES ('root', '123456', 1);
```

##### 在mysql创建监控用户（用户名固定monitor，如果需要修改，则要在proxysql修改，命令：UPDATE global_variables SET variable_value='新的监控用户名' WHERE variable_name='mysql-monitor_username';）

```
#(完整案例：CREATE USER 'monitor'@'192.%' IDENTIFIED WITH mysql_native_password BY '123456';)
CREATE USER 'monitor'@'替换为 ProxySQL 服务器的 IP 地址或主机名' IDENTIFIED BY '替换为你想要设置的密码'; 
GRANT SELECT, REPLICATION CLIENT ON *.* TO 'monitor'@'proxy_sql_ip'; (完整案例：GRANT SELECT, REPLICATION CLIENT ON *.* TO 'monitor'@'192.%';)
FLUSH PRIVILEGES;
```

##### 修改proxysql的监控账户密码

```
UPDATE global_variables SET variable_value='123456' WHERE variable_name='mysql-monitor_password';
```


##### 添加心跳检测（120秒一次）

```
INSERT INTO mysql_servers(hostgroup_id, hostname, port, monitor_username, monitor_password, monitor_interval_ms, monitor_query) VALUES (1, '192.168.0.12', 3306, 'monitor', '123456', 120000, 'SELECT 1');

```

###### 心跳断开之后，如果报这个错误：

```
[ERROR] Failed to mysql_real_connect() on 1:192.168.0.12:3306 , FD (Conn:48 , MyDS:48) , 1129: Host '192.168.0.27' is blocked because of many connection errors; unblock with 'mysqladmin flush-hosts'
```

##### 解决方案：

```
1.执行sql语句： flush hosts
或者：
1.进入mysql主机，在mysql/bin的目录下执行这个命令： mysqladmin -u root -p flush-hosts
2.系统会提示您输入 root 用户的密码。

#设置mysql自动解除阻止：
1.编辑 MySQL 配置文件，通常是 /etc/my.cnf 或 /etc/mysql/my.cnf，具体位置取决于您的系统和安装方式。
2.在 [mysqld] 部分添加以下行来调整参数：
[mysqld]
max_connect_errors = 3000  # 设置为您希望的连接错误次数阈值
connect_timeout = 10       # 设置为连接超时的时间（单位：秒）
3.重启 MySQL 服务，以使更改生效：
```

解释：
在 MySQL 中，当一个主机的连接错误次数达到 max_connect_errors 参数设置的阈值后，MySQL 会自动将该主机阻止（block）。被阻止的主机会在 connect_timeout 参数设置的时间间隔后自动解除阻止。也就是说，当一个主机达到连接错误次数上限后，在 connect_timeout 定义的时间内，该主机将无法连接到 MySQL 服务器。

例如，如果您将 max_connect_errors 设置为 1000，connect_timeout 设置为 10（单位：秒），那么当一个主机的连接错误次数达到 1000 后，该主机将在接下来的 10 秒内被阻止。在这 10 秒内，该主机无法连接到 MySQL 服务器。过了这个时间，如果该主机再次尝试连接，MySQL 会将其解除阻止，允许其重新连接。

这种自动解除阻止的机制可以确保即使主机由于连接错误被阻止，也有一个自动恢复的机制，使得在一定时间后该主机可以再次尝试连接。这样有助于避免永久阻止合法的客户端，同时也是一种防范恶意连接尝试的安全措施。

proxysql保存命令：

```
LOAD MYSQL SERVERS TO RUNTIME;
SAVE MYSQL SERVERS TO DISK;
LOAD MYSQL USERS TO RUNTIME;
SAVE MYSQL USERS TO DISK;
LOAD MYSQL VARIABLES TO RUNTIME;
SAVE MYSQL VARIABLES TO DISK;
```

