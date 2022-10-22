### nginx 限流

1. 漏桶算法实现控制速率限流

​	漏桶算法的思路很简单，请求先进入漏桶内，漏桶以一定的速率出水，当进来的请求太大，漏桶则会溢出，然后就拒绝请求

![在这里插入图片描述](https://www.freesion.com/images/11/5bc49dbcbc6f09d1a21a5496c8195a73.png)

​	配置：nginx.conf

```nginx
http{
	...
	#限流设置 binary_remote_addr:根据单个客户的请求ip进行限流 contentRateLimit:缓存空间名称 10m：缓存空间（1M可以存储16000个IP信息） 				rate=2r/s:每秒允许两个请求
	limit_req_zone $binary_remote_addr zone=contentRateLimit:10m rate=2r/s
	
	#站点配置
	server {
		listen 80;
		server_name localhost;
		
		location /api {
			#使用限流配置:limit_req zone=缓存空间名称;  burst=4:若同时有4个突发请求到达，nginx会处理第一个请求，剩下的请求放进队列，然后每隔500ms处理一个请求，若请求总数大于4，则会拒绝多余的请求，直接返回503；nodelay：请求并行处理，平均每秒不超过2个请求，突发请求不超过4个，则所有请求同时处理
			limit_req zone=contentRateLimit burst=4 nodelay;
            ...
		}
	}
}
	
```



2.控制并发量



```nginx
http{
	...
     #限流设置 binary_remote_addr:根据单个客户的请求ip进行限流 contentRateLimit:缓存空间名称 10m：缓存空间（1M可以存储16000个IP信息） 				rate=2r/s:每秒允许两个请求
	limit_req_zone $binary_remote_addr zone=contentRateLimit:10m rate=2r/s
        
    #对用户IP进行并发计数，将计数内存区命名为perip，设置计数内存区大小为10MB （1M可以存储16000个IP信息）
    limit_conn_zone $binary_remote_addr zone=perip:10m;
    #整个location对应的请求的并发容量配置
    limit_conn_zone $server_name zone=perserver:10m;
    server{
        listen 80;
        server_name localhost;
        location /api {
            
            limit_req zone=contentRateLimit burst=4 nodelay;
            
            #个人IP限流配置, 单个客户端ip与服务器的连接数限制为3
            limit_conn perip 3;
            #当前location的总并发配置,总并发数100
            limit_conn perserver 100;
            ...
        }
    }
}
```



