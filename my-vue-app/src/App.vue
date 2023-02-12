<template>
  <div class="table-box">
    <!-- 标题 -->
    <div class="title">
      <h2>CRUD</h2>
    </div>
    <!-- query -->
    <div class="query-box">
      <el-input class="query-input" v-model="queryInput" placeholder="请输入姓名搜索" @input="handleQueryName"/>
      <div class="btn-list">
        <el-button type="primary" @click="handleAdd">增加</el-button>
      <el-button type="danger" @click="handleDelList" v-if="multipleSelection.length>0">删除多选</el-button>
      </div>
    </div>
    <!-- table -->
    <el-table  
      ref="multipleTableRef"
      :data="tableData"
      style="width: 100%"
      @selection-change="handleSelectionChange" 
      border
    >
    <el-table-column type="selection" width="55" />
    <el-table-column prop="name" label="姓名" width="120" />
    <el-table-column prop="email" label="邮箱" width="120" />
    <el-table-column prop="phone" label="电话" width="120" />
    <el-table-column prop="state" label="状态" width="120" />
    <el-table-column prop="address" label="地址" width="300"/>
    <el-table-column fixed="right" label="操作" width="120">
      <template #default="scope">
        <el-button link type="primary" size="small" @click="handleRowDel(scope.row)" style="color: #F56C6C">删除</el-button>
        <el-button link type="primary" size="small" @click="handleEdit(scope.row)">编辑</el-button>
      </template>
    </el-table-column>
  </el-table>
  <!-- dialog弹窗 -->
  <el-dialog v-model="dialogFormVisible" :title="dialogType === 'add' ? '新增' : '编辑'">
    <el-form :model="tableForm">
      <el-form-item label="姓名" :label-width="80">
        <el-input v-model="tableForm.name" autocomplete="off" />
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
        <el-button type="primary" @click="dialogConfirm">
          确认
        </el-button>
      </span>
    </template>
  </el-dialog>
  </div>
</template>

<script setup>
import {ref} from 'vue';
// 数据
  let queryInput = $ref("")
  let multipleSelection = $ref([])
  let dialogFormVisible = $ref(false)
  let dialogType = $ref('add')
  let tableForm = $ref({
    name: '张三',
    email: '123@qq.com',
    phone: '12345678900',
    state: '出差',
    address: '吉林省'
  })
  let tableData = $ref([
  {
    id:'1',
    name: 'Tom1',
    email: '123@qq.com',
    phone: '12345678900',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id:'2',
    name: 'Tom2',
    email: '123@qq.com',
    phone: '12345678900',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id:'3',
    name: 'Tom3',
    email: '123@qq.com',
    phone: '12345678900',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id:'4',
    name: 'Tom4',
    email: '123@qq.com',
    phone: '12345678900',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
])
  let tableDataCopy = Object.assign(tableData)
// 方法
// 搜索
const handleQueryName = (val) => {
  // console.log(queryInput)
  // console.log(val)
  if(val.length > 0){
    tableData = tableData.filter(item => (item.name).toLowerCase().match(val.toLowerCase()))
  }else{
    tableData = tableDataCopy
  }
  
}
// 编辑
const handleEdit = (row) => {
  dialogFormVisible = true
  dialogType = 'edit'
  console.log(row)
  tableForm = {...row}
}
// 删除一条
const handleRowDel = ({id}) => {
  console.log(id)
  // 1.通过id获取到条目对应的索引值
  let index = tableData.findIndex(item => item.id === id)
  // 2.通过索引值进行删除对应条目
  tableData.splice(index, 1)
}
const handleDelList = () => {
  multipleSelection.forEach(id => {
  handleRowDel({id})
  })
  multipleSelection=[]
}
// 选中
const handleSelectionChange = (val) => {
  // multipleSelection = val
  // console.log(val)
  multipleSelection=[]
  val.forEach(item => {
    multipleSelection.push(item.id)
  })
  console.log(multipleSelection)
}
//新增 
const handleAdd = () => {
  dialogFormVisible = true
  // console.log(dialogFormVisible)
  tableForm = {}
  dialogType = 'add'
}
// 确认
const dialogConfirm = () => {
  dialogFormVisible = false
  // 判断是新增还是编辑
  if(dialogType === 'add'){
   // 1.拿到数据
  // 2.添加到table
  tableData.push({
    id: (tableData.length + 1).toString(),
    ...tableForm
  })
 }else{
  // 1.获取到当前这条的索引
  let index = tableData.findIndex(item => item.id === tableForm.id)
  console.log(index)
  // 2.替换当前索引值对应的数据
  tableData[index] = tableForm
 } 
}
</script>

<style scoped>
.table-box{
  width: 800px;
  margin: 100px auto;
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
  width: 200px;
}
</style>