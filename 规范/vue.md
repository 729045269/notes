#### vue开发规范

1. 所有页面上的变量必须预定义出来，禁止使用直接复制变量来定义

   ```
   正确写法：
    data() {
        return {
        	form: {
        		id:'',
        		name: ''
        	},
        }
    },
    错误写法：
     data() {
        return {
        	form: {},
        }
    },
   ```

   