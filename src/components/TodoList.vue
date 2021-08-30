<template>
  <div>
    <el-card>
      <el-row :gutter="20">
        <el-col :span="16" :offset="2">
          <el-input v-model="newTodo" placeholder="请输入待办事项..."></el-input>
        </el-col>
        <el-col :span="6">
          <el-button type="primary" icon="el-icon-plus">新建</el-button>
        </el-col>
      </el-row>
      <el-divider></el-divider>
      <el-table :data="tableData" style="width: 100%" :row-class-name="tableRowClassName" border>
        <el-table-column type="index" width="80"></el-table-column>
        <el-table-column align="center" label="待办事项"></el-table-column>
        <el-table-column align="center" label="操作">
          <template>
            <el-button
              type="success"
              icon="el-icon-check"
              circle
            ></el-button>
            <el-button
              type="danger"
              icon="el-icon-close"
              circle
            ></el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
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
      async getTodoList () {
        const { data: res } = await this.$http.get('todo')
        if (res.meta.status !== 200) {
          return this.$message.error('获取清单失败')
        }
        this.tableData = res
      },
      tableRowClassName (row) {
        if (row.status) {
          return 'success-row'
        } else {
          return ''
        }
      }
    }
  }
</script>

<style>
  .el-table .success-row {
    text-decoration: line-through;
  }
</style>
