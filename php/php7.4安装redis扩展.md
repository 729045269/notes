# php7.4安装redis扩展

官网下载redis扩展： http://pecl.php.net/get/redis-5.3.5.tgz

```
我这里下载的是5.3.5版本的redis扩展

进入目录： cd /root

下载： wget http://pecl.php.net/get/redis-5.3.5.tgz

解压：tar -xvf redis-5.3.5.tgz

进入目录： cd redis-5.3.5

执行命令编译：

  /usr/local/php/bin/phpize

  ./configure --with-php-config=/usr/local/php/bin/php-config

  make && make install

修改php.ini,添加扩展:

  extension = redis.so

重启php服务：systemctl php-fpm restart

查看扩展是否安装成功： php -m

   
```

