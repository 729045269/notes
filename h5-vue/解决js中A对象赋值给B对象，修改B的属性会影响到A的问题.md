#### 解决js中A对象赋值给B对象，修改B的属性会影响到A的问题

例：this.A = this.B  或  const A = this.B  等形式的 对象赋值，修改新对象A导致原对象B同步变化

【问题原因】：

赋值时没有创建一个新的对象内存地址；

只是把 B 的内存地址指向了 A 的内存地址，一旦  B 值发生改变，但内存地址不会发生改变，所以A 的值也会相对应改变

没有进行深度拷贝，只是把this.A的地址指向了与this.B相同的地址，两者共用同一内存，所以修改this.A导致this.B同步变化。

例如：

```
let origin = {
	name: "张三",
	age: 24
}

let target = origin
target.name = "李四"  //origin的name属性变成了“李四”
```

上面的代码将target对象的属性修改之后，**origin也会相应的改变**，因为这里的target与origin这两个引用实际上是指向同一个对象。

解决方法：

1. ##### 深度拷贝, `Object.assign()` 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。**

   ```
   let origin = {
   	name: "张三",
   	age: 24
   }
   
   let target = Object.assign({},origin)
   target.name = "李四" //origin的name属性不会发生变化
   ```

2. ##### 将对象转成json字符串再转回来

   ```
   let origin = {
   	name: "张三",
   	age: 24
   }
   let target =  JSON.parse(JSON.stringify(origin))
   target.name = "李四" //origin的name属性不会发生变化
   ```

3. ##### 使用 扩展运算符 转换后再赋值

   ```
   let origin = {
   	name: "张三",
   	age: 24
   }
   let target = {...origin}
   target.name = "李四" //origin的name属性不会发生变化
   ```

   

