#### iphone在html中关闭input首字母大写

```
<input type="text" autocomplete="off" autocapitalize="off" autocorrect="off"/>

autocomplete  
　　默认为on，其含义代表是否让浏览器自动记录自谦输入的值。
　　很多时候，需要对客户的资料进行保密，防止浏览器软件或者恶意插件获取到。可以在input中加入autocomplete = "off"来关闭记录，系统需要保密的情况下可以使用此参数。

autocapitalize 
　　自动大小写

autocorrect
　　纠错

```

