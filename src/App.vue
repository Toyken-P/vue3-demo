<script setup>
  import {ref} from "vue";

  /*数据*/
  let queryInput = ref("")
  let tableData = ref([
    {
      id: "1",
      name: 'Tom',
      phone: '19233456871',
      email: '795461@qq.com',
      state: '在职',
      address: 'No. 189, Grove St, Los Angeles',
    },
       {
      id: "2",
      name: 'Jack',
      phone: '19233456871',
      email: '795461@qq.com',
      state: '在职',
      address: 'No. 189, Grove St, Los Angeles',
    },    {
      id: "3",
      name: 'Alice',
      phone: '19233456871',
      email: '795461@qq.com',
      state: '在职',
      address: 'No. 189, Grove St, Los Angeles',
    },    {
      id: "4",
      name: 'Lucy',
      phone: '19233456871',
      email: '795461@qq.com',
      state: '在职',
      address: 'No. 189, Grove St, Los Angeles',
    },
  ])
  let multipleSelection = ref([])
  let dialogFormVisible = ref(false)
  let tableForm = ref({
    name:'张三',
    email:"416465@qq.com",
    phone:"146462486",
    state:"在职",
    address:"江苏省"
  })
  let dialogType = ref('add')

  /*方法*/
  // 删除一条
  const handleRowDel = ({id}) => {
    console.log(id)
    // 1.通过id获取到条目对应索引值
    let index = tableData.value.findIndex(item=>item.id === id)
    console.log(index)
    // 2. 通过索引值删除对应条目
    tableData.value.splice(index,1)
  }

  const handleDelList = () =>{
    multipleSelection.value.forEach(id =>(
      handleRowDel({id})
    ))
    multipleSelection.value=[]
  }

  // 选中
  const handleSelectionChange = (val) => {
  // multipleSelection.value = val
  // console.log(val)
  multipleSelection.value = []
    val.forEach(item => {
      multipleSelection.value.push(item.id)
    })
    console.log(multipleSelection.value);
  }

  // 新增
  const handleAdd = () =>{
    dialogFormVisible.value = true
    tableForm.value = {}
  }
  // 确认添加
  const dialogConfirm = () =>{
    dialogFormVisible.value = false
    // 1.拿到数据

    // 2.添加到table
    tableData.value.push({
      id: (tableData.value.length + 1).toString(),
      ...tableForm.value
    })
    console.log(tableData)
  }
</script>

<template>
  <div class="table-box">

    <!-- 标题 -->
    <div class="title">
      <h2> 简单的crud Demo </h2>
    </div>

    <!-- query-->
    <div class="query-box">
      <el-input class= "query-input" v-model="queryInput" placeholder="请输入姓名搜索"/>
      <div class="button-list">
        <el-button type="primary" @click="handleAdd">增加</el-button>
        <el-button type="danger" @click="handleDelList" v-if="(multipleSelection.length) > 0">
        多选删除
        </el-button>
      </div>
    </div>

    <!--table-->
    <el-table ref="multipleTableRef"
    :data="tableData"
    style="width: 100%"
    @selection-change="handleSelectionChange" 
    border
    >
     <el-table-column type="selection" width="55" />
      <el-table-column prop="name" label="姓名" width="150" />
      <el-table-column prop="phone" label="电话" width="120" />
      <el-table-column prop="email" label="邮箱" width="200" />
      <el-table-column prop="state" label="状态" width="120" />
      <el-table-column prop="address" label="地址" width="300" />
      <el-table-column fixed="right" label="操作" width="120">
        <template #default="scope">
          <el-button link type="primary" size="small" @click="handleRowDel(scope.row)" style="color:#F56C60">删除</el-button>
          <el-button link type="primary" size="small">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!--dialog -->
    <el-dialog v-model="dialogFormVisible" :title="dialogType === 'add' ? '新增' : '编辑'" center width="30%" >
    <el-form :model="tableForm">
      <el-form-item label="姓名" :label-width="80">
        <el-input v-model="tableForm.name" autocomplete="off"/>
      </el-form-item>
      <el-form-item label="邮箱" :label-width="80">
        <el-input v-model="tableForm.email" autocomplete="off" />
      </el-form-item>
      <el-form-item label="电话" :label-width="80">
        <el-input v-model="tableForm.phone" autocomplete="off" />
      </el-form-item>
      <el-form-item label="状态" :label-width="80">
        <el-input v-model="tableForm.state" autocomplete="off" />
      </el-form-item>
      <el-form-item label="地址" :label-width="80">
        <el-input v-model="tableForm.address" autocomplete="off" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取消</el-button>
        <el-button type="primary" @click="dialogConfirm">
          确认
        </el-button>
      </span>
    </template>
  </el-dialog>
  </div>
</template>

<style scoped>
  .table-box{
    width: 800px;
    margin: 200px auto;
  }
  .title{
    text-align: center;
  }
.query-box{
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}
.query-input{
  width:200px;
}

</style>
