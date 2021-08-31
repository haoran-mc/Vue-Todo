<template>
  <!-- 卡片区域 -->
  <el-card style="width: 60%; margin-left: 20%; margin-right: 20%">
    <!-- 新建Todo项 -->
    <el-row :gutter="20">
      <el-col :span="16" :offset="2">
        <el-input v-model="newTodo" placeholder="请输入待办事项..."></el-input>
      </el-col>
      <el-col :span="6">
        <el-button type="primary" icon="el-icon-plus" @click="TodoAdd">新建</el-button>
      </el-col>
    </el-row>
    <!-- 分割线 -->
    <el-divider></el-divider>
    <!-- 表格区域 -->
    <el-table :data="tableData" border :row-class-name="TodoState">
      <el-table-column type="index" width="50"></el-table-column>
      <el-table-column align="center" label="待办事项" prop="title"></el-table-column>
      <el-table-column align="center" label="操作" width="150">
        <template slot-scope="scope">
          <el-button
            type="success"
            icon="el-icon-check"
            v-show="!scope.row.status"
            @click="TodoEdit(scope.$index, scope.row)"
            circle
          ></el-button>
          <el-button
            type="warning"
            icon="el-icon-refresh-left"
            v-show="scope.row.status"
            @click="TodoEdit(scope.$index, scope.row)"
            circle
            style="margin: 0;"
          ></el-button>
          <el-button
            type="danger"
            icon="el-icon-close"
            @click="TodoDelete(scope.$index, scope.row.id)"
            circle
          ></el-button>
        </template>
      </el-table-column>
    </el-table>
  </el-card>
</template>

<script>
  export default {
    data () {
      return {
        tableData: [],
        newTodo: ''
      }
    },
    mounted () {
      this.getTodoList()
    },
    methods: {
      getTodoList () {
        this.axios
            .get('/v1/todo')
            .then(response => (this.tableData = response.data))
      },
      TodoAdd () {
        if (this.newTodo !== '') {
          this.axios.post('/v1/todo', { title: this.newTodo }).then(() => {
                this.getTodoList()
                this.$message.success({
                  showClose: true,
                  duration: 1500,
                  message: '添加待办事项成功！'
                })
              })
          this.newTodo = ''
        } else {
          this.$message.warning({
            showClose: true,
            duration: 1500,
            message: '待办事项不能为空！'
          })
        }
      },
      TodoEdit (index, row) {
        const msg = row.status ? ' 置为未完成' : ' 置为已完成'
        this.axios.put('/v1/todo/' + row.id, { status: !row.status }).then(() => {
              this.tableData[index].status = !row.status
              this.$message.success({
                showClose: true,
                duration: 1500,
                message: `<${row.title}> ${msg}`
              })
            })
      },
      TodoDelete (index, id) {
        this.axios.delete('/v1/todo/' + id).then(() => {
          this.tableData.splice(index, 1)
          this.$message.success({
            showClose: true,
            duration: 1500,
            message: '删除待办事项成功！'
          })
        })
      },
      TodoState ({ row }) {
        if (row.status) {
          return 'TodoFinish'
        } else {
          return ''
        }
      }
    }
  }
</script>

<style>
  .el-table .warning-row {
    background: oldlace;
    margin: 0;
  }

  .el-table .TodoFinish {
    text-decoration: line-through;
  }
</style>
