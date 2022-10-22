# centos安装redis

1. 进入到目录：cd /usr/local
2. 下载安装包  wget https://download.redis.io/releases/redis-6.2.6.tar.gz
3. 执行命令

```
检查是否安装了gcc环境： gcc -v

安装gcc： yum -y install gcc-c++

解压： tar -xvf redis-6.2.6.tar.gz

将文件夹重命名： mv redis-6.2.6.tar.gz reids

进入到redis文件夹： cd redis

编译安装：make && make install

```

​	4.修改redis的保护模式和后台运行（可选）

```
vim /usr/local/redis/redis.conf

daemonize no #Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程

protected-mode no #将这个参数改成no
```

5. 启动redis： /usr/local/redis/src/redis-server /usr/local/redis/redis.conf &