<!-- todolist主体部分 -->
<template>
  <div class='todolist-main'>
    <div class="todolist-wrap" v-for="(todoList, index) in todoLists" :key="index">
      <TodoListItem 
      :todoList="todoList"
      :index="index"
      />
    </div>
  </div>
</template>

<script>
import TodoListItem from './TodoListItem'
export default {
  components: {
    TodoListItem
  },
  props: {
    todoLists: Array
  },
  methods: {
  }
}
</script>
<style lang='scss' scoped>
.todolist-main {
  position: relative;
  width: 100%;
  .todolist-wrap {
    position: relative;
  }
}
</style>