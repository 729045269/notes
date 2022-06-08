#### vue(element ui)给table里每个按钮设置loading效果

这里以element ui为例，其他框架同理

```
<template>
    <el-table :data="list" border fit>
        <el-table-column label="ID" prop="id" align="center"></el-table-column>
        <el-table-column label="名称" prop="name" align="center"></el-table-column>
        <el-table-column label="操作" align="center">
            <template slot-scope="scope">
                <el-button type="primary" size="mini" :loading="scope.row.loading" @click="handleEdit(scope.row)">
                	编辑
                </el-button>
            </div>
            </template>
        </el-table-column>
    </el-table>
</template>
<script>
export default {
    data() {
    return {
		list: [
			{id:1,name:"张三"},
			{id:2,name:"李四"},
			{id:3,name:"王五"},
		]
    }
    },
    created() {
   
    },
    methods: {
        //编辑
        handleEdit(row) {
            this.$set(row,"loading",true)
            setTimeOut(()=>{
            	this.$set(row,"loading",false)
            },1000)
        }
    },

    }
<script>
```

