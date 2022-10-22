### centos安装nginx

```
首先进到opt目录: cd /opt
1.下载安装包：  wget http://nginx.org/download/nginx-1.21.6.tar.gz  
2.解压： tar -zvxf nginx-1.21.6.tar.gz 


预编译
进入解压后的目录 nginx-1.21.6 进行预编译  ./configure --prefix=/opt/nginx 注意：--prefix是指定安装目录（建议指定）
1.进入解压目录： cd nginx-1.21.6
2.预编译： ./configure --prefix=/opt/nginx
```

如果提示下面的错误，请按照相相对应的解决方法

- 错误1：

```
checking for C compiler … not found
./configure: error: C compiler cc is not found
```

- 解决方法：

```
yum -y install gcc gcc-c++ 
```



- 错误2：

```
./configure: error: the HTTP rewrite module requires the PCRE library.    
You can either disable the module by using --without-http_rewrite_module            
option, or install the PCRE library into the system, or build the PCRE library   
statically from the source with nginx by using --with-pcre=<path> option.
```

- 解决方法：

```
 yum -y install pcre-devel 
```



- 错误3：

```
./configure: error: the HTTP gzip module requires the zlib library.    
You can either disable the module by using --without-http_gzip_module         
option, or install the zlib library into the system, or build the zlib library       
statically from the source with nginx by using --with-zlib=<path> option.
```

- 解决方法： 

```
yum -y install zlib-devel 
```



预编译完之后，输出的内容是这样子的：

```
Configuration summary
  + using system PCRE library
  + OpenSSL library is not used
  + using system zlib library
  
  nginx path prefix: "/opt/nginx"
  nginx binary file: "/opt/nginx/sbin/nginx"
  nginx modules path: "/opt/nginx/modules"
  nginx configuration prefix: "/opt/nginx/conf"
  nginx configuration file: "/opt/nginx/conf/nginx.conf"
  nginx pid file: "/opt/nginx/logs/nginx.pid"
  nginx error log file: "/opt/nginx/logs/error.log"
  nginx http access log file: "/opt/nginx/logs/access.log"
  nginx http client request body temporary files: "client_body_temp"
  nginx http proxy temporary files: "proxy_temp"
  nginx http fastcgi temporary files: "fastcgi_temp"
  nginx http uwsgi temporary files: "uwsgi_temp"
  nginx http scgi temporary files: "scgi_temp"
```



开始安装1`

```
编译： make 
安装： make install 

安装结束后进入预编译指定的目录： cd /opt/nginx
查看nginx版本号： ./sbin/nginx -v
启动:	./sbin/nginx
停止:	./sbin/nginx -s stop
安全退出：	./sbin/nginx -s quit 
重载:	./sbin/nginx -s reload
查看nginx进程:	ps aux|grep nginx
```

