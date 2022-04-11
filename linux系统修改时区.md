linux系统修改时区

先查看时区：

```
[root@localhost ~]# cat /etc/sysconfig/clock
上面的命令会输出下面的内容：
ZONE="America/New_York"
```

然后选择要修改的时区

```
[root@localhost ~]# tzselect
然后根据自己的需要选择时区： Asia，china，shanghai

选择时区之后，查看一下时区是否正确
[root@localhost ~]# cat /etc/sysconfig/clock
输出：
ZONE="Asia/Shanghai"
UTC=true

```

删除旧的时区配置：

```
[root@localhost ~]# rm /etc/localtime
```

软链新的时区配置：

```
/usr/share/zoneinfo/ 这个目录里面有所有时区，自己选择需要的时区更新进去
[root@localhost ~]# ln -s /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
```

重启系统

```
[root@localhost ~]# reboot
查看时间是否正确：
[root@localhost ~]# date
```

