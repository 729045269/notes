# vuex 模块化之后如何调用其他模块的方法和属性

**在一个模块的actions中调用其他模块的actions**

```
dispatch('setting/height', 100, {root: true}) 
```



**在一个模块如果想触发其他模块的mutation动态更新state**

```
 commit('setting/width', 100, {root: true})
```



