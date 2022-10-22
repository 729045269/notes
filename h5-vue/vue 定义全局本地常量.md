# vue 定义全局本地常量

新建本地常量文件constant.js

```
export default {
  ORDER_STATUS_NAME:{
    0: "等待",
    1: "成功",
    2: "失败",
  }
}

```

在项目main.js里引入并添加到

```
...

//全局常量
import constant from "./constant.js"

Vue.prototype.$CONSTANT = constant

...

```

在页面中使用

```
<template>
	<div class="app-container">
      <el-form>
          <el-form-item label="状态" prop="status">
              <el-select v-model="queryParams.status">
                  <el-option 
                  v-for="(item,index) in $CONSTANT.ORDER_STATUS_NAME"
                  :label="item"
                  :value="index"
                  />
              </el-select>
          </el-form-item>
      </el-form>
	</div>
</template>
<script>
	export default {
		data() {
	    	return {
	    		queryParams:{
	    			status:""
	    		}
	    	}
    	}
		created() {
         	console.log(this.$CONSTANT.ORDER_STATUS_NAME)   
        },
	}
</script>
```

