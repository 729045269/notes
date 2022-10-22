### centos安装RabbitMq

```
安装 RabbitMq：
yum install rabbitmq-server 
service rabbitmq-server  start


设置为开机启动：
chkconfig rabbitmq-server on


开启web界面管理工具：
rabbitmq-plugins enable rabbitmq_management
service rabbitmq-server restart

装完之后，需要开放15672端口，默认访问地址：ip:15672 ,rabbitmq提供的默认账号密码都是guest


创建一个新的用户：
rabbitmqctl add_user root 123456

# 创建好之后，给该用户管理员的角色
rabbitmqctl set_user_tags root administrator


其他命令：
重启：service rabbitmq-server restart
关闭： service rabbitmq-server stop
查看状态： rabbitmqctl status

```