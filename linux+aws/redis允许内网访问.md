#### redis允许内网访问

允许远程访问：确认 # bind 127.0.0.1 这一句处于注释状态，如去掉注释，则只能本地访问

设置密码：# requirepass foobared 找到这一句，修改requirepass 后面的密码，并将注释去掉

修改端口：redis默认的端口是6379，找到 port 6379 这一行，修改端口