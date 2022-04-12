# centos7安装docker

1.安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

2.设置yum源

```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

3.安装

```
yum install docker-ce 
```

4.启动并加入开机启动

```
systemctl start docker

systemctl enable docker
```

5.验证安装是否成功

```
docker version
```

创建容器：

```
docker run -d --name php-nginx-test -p 9100:80 -v /e/docker-root/:/app webdevops/php-nginx  

-d: 放入后台运行，-p: 指定端口映射关系（第一个为宿主机端口、第二个为容器端口）,-v：文件映射，第一个是宿主机的目录路径冒号后面的是容器的路径（开启映射需要在docker的设置中将Resources=》File sharing里面的路径配置好）
```

停止容器：docker stop 容器id

启动容器：docker start 容器id