<template>
  <el-container class="todo-container">
    <!-- 头部区域 -->
    <el-header>Todo清单</el-header>
    <!-- 页面主体区域 -->
    <el-main>
      <el-row>
        <el-col>
          <TodoList></TodoList>
        </el-col>
      </el-row>
    </el-main>
    <el-footer>Gin框架小练习</el-footer>
  </el-container>
</template>

<script>
  // @ is an alias to /src
  import TodoList from '@/components/TodoList.vue'

  export default {
    components: {
      TodoList
    }
  }
</script>

<style>
  .todo-container {
    height: 100%;
  }

  .el-header {
    background-color: #8b8b8b;
    color: #fff;
    text-align: center;
    line-height: 60px;
  }

  .el-footer {
    background-color: #8b8b8b;
    color: #fff;
    text-align: right;
    line-height: 60px;
  }
</style>
