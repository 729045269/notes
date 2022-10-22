# php向redis list一次性lPush多个值

该功能使用可变参数可以实现，首先查看一下redis插件的代码

![image-20220718021913987](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220718021913987.png)

具体实现代码：

```
$redis = new Redis();
$redis->connect("127.0.0.1", 6379);
$key = "test-list";
$list = [];
for ( $i=0; $i<10; $i++ ) {
	$list[] = $i;
}
$redis->lPush($key, ...$list);
```

查看最后的执行结果，如下图：

![image-20220718022350942](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220718022350942.png)