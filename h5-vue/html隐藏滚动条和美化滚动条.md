### html隐藏滚动条和美化滚动条

```html
1. 通过控制css的属性overflow来实现隐藏滚动条的效果，但是这个的弊端就是隐藏滚动条的同时页面也不能滚动了
    html { 
        overflow-x :hidden; /*隐藏水平滚动条*
        overflow-y :hidden; /*隐藏垂直滚动条*
    }

2.通过伪元素方法将滚动条的宽度设置为0，下面的例子以垂直滚动条为例（水平滚动条同理）
    html::-webkit-scrollbar {
      width: 0;
    }

```



##### 美化滚动条

```
.main::-webkit-scrollbar {
    width:8px;
    height:8px;
}

/*滚动条里面小方块样式*/

.main::-webkit-scrollbar-thumb {
    border-radius:100px;
    -webkit-box-shadow:inset 0 0 5px rgba(0,0,0,0.2);
    background:red;
}

/*滚动条里面轨道样式*/
.main::-webkit-scrollbar-track {
    -webkit-box-shadow:inset 0 0 5px rgba(0,0,0,0.2);
    border-radius:0;
    background:rgba(0,0,0,0.1);
}
```

