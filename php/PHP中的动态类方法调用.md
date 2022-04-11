**PHP中的动态类方法调用**



```js
$this->{$methodName}($arg1, $arg2, $arg3);
$this->$methodName($arg1, $arg2, $arg3);
call_user_func_array(array($this, $methodName), array($arg1, $arg2, $arg3));
```