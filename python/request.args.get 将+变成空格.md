# request.args.get 将+变成空格

## 解决方法一：

1、客户端发起请求时特殊字符转ASCII码

2、服务端收到参数值后，遇到有空格的地方，把特殊字符再丢进去

代码：```input = request.args.get('input')```

```
input = input.replace(' ','+') # 密文中含有符号+，URL转义，会出现get请求传值为空的情况，需要把参数中的空格替换成原本的符号+
```





## 解决方法二：

将url请求的参数进行urlencode编码处理