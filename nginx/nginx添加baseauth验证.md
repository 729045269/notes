### nginx添加baseauth验证

**确定是否安装了httpd-tools(使用命令 `htpasswd --help` )，没有安装则执行下面命令**

```
yum install httpd-tools -y
```

#### 语法

#### htpasswd(选项)(参数)

```
-c：创建一个加密文件；
-n：不更新加密文件，只将加密后的用户名密码显示在屏幕上；
-m：默认采用MD5算法对密码进行加密；
-d：采用CRYPT算法对密码进行加密；
-p：不对密码进行进行加密，即明文密码；
-s：采用SHA算法对密码进行加密；
-b：在命令行中一并输入用户名和密码而不是根据提示输入密码；
-D：删除指定的用户。
-B：强制对密码进行bcrypt加密（非常安全）。
-C：设置用于bcrypt算法的计算时间
-v：验证指定用户的密码。
```

**创建授权用户和密码**

```
htpasswd -c -d /etc/nginx/conf/pass_file username(用户名)
然后会提示输入密码
New password: 123456
Re-type new password: 123456
```

**利用htpasswd命令修改密码**

```
htpasswd -D /etc//nginx/conf/pass_file username
htpasswd -b /etc/nginx/nginx/conf/pass_file username newpassword(新密码)
```



#### 配置nginx

```
server {
  listen    80;  
  server_name local;
 
  auth_basic  "登录认证"; 
  #auth_basic 设置为off则不开启验证
  #auth_basic off;
  #这个路径必须正确，否则会访问不到资源，一直返回403错误
  auth_basic_user_file /etc/nginx/conf/pass_file;
 
  root  /wwwroot/html/xxx;
  index index.html index.php;
}
```

