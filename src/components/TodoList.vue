<!--todolist根组件-->
<template>
<div class='todolist-root'>
  <div class="section" >
    <TodoListHeader/>
    <TodoListInput @entertoadd='addToLists'/>
    <TodoListItems 
    :todoLists = "todoLists"/>  
    <TodoListFooter
    :todoLists = "todoLists"/>
  </div>
</div>
</template>

<script>
import TodoListHeader from './TodoListHeader'
import TodoListInput from './TodoListInput'
import TodoListItems from './TodoListItems'
import TodoListFooter from './TodoListFooter'
export default {
  data() {
    //存放数据
    return {
      todoLists: []
    };
  },
  components: {
    TodoListHeader,
    TodoListInput,
    TodoListItems,
    TodoListFooter
  },
  //方法集合
  methods: {
    addToLists(data) {
      this.todoLists.push(data)
    },
    deleteItem(index) {
      this.todoLists.splice(index, 1)
    },
    editItem(index, newValue) {
      this.todoLists.splice(index, 1, newValue)
    }
  }
}
</script>
<style lang='scss' scoped>
.todolist-root {
  position: relative;
  margin: 130px auto 0px;
  width: 600px;
  height: auto;
  border: none;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
}
</style>