# Nginx 配置按日期每天生成一个日志文件

```
在http中添加下面的内容

http{
	...
	map $time_iso8601 $logdate  {
      '~^(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})' $2$3; #获取到的格式是 月份+日，例如：0503
      #'~^(?<ymd>\d{4}-\d{2}-\d{2})' $ymd;  #获取到的格式是 年月日，例如：20230503
      default '';
    }
	...
	
	server{
		...
		access_log /www/wwwlogs/cnblogs.com/access-$logdate.log;
	}
}
```

