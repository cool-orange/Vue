<!--todolist根组件-->
<template>
<div class='todolist-root'>
  <div class="section" >
    <TodoListHeader/>
    <TodoListInput @entertoadd='addToLists'/>
    <TodoListItems 
    :checkedBtn = "checkedBtn"
    :todoLists = "todoLists"
    @completedone='listsAllDone'/>  
    <TodoListFooter
    :todoLists = "todoLists"
    :activeListsLength = "activeListsLength"
    :checkedBtn = "checkedBtn"
     @filter='filterLists'
     @clear='clearCompletedLists'/>
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
      //原数据
      todoLists: [],
      //底部选中按钮
      checkedBtn: 'all'
    };
  },
  components: {
    TodoListHeader,
    TodoListInput,
    TodoListItems,
    TodoListFooter
  },
  computed: {
    activeListsLength() {
      return this.todoLists.filter(item => item.active).length
    }
  },
  //方法集合
  methods: {
    //添加
    addToLists(data) {
      this.todoLists.push({
        data: data,
        active: true
      })
    },
    //删除
    deleteItem(index) {
      this.todoLists.splice(index, 1)
    },
    //更新
    updateItem(index, key, data) {
      if(data === '') {
        this.deleteItem(index)
        return
      }
      this.todoLists[index] && this.$set(this.todoLists[index], key, data)  
    },
    //过滤
    filterLists(key) {
      this.checkedBtn = key
    },
    listsAllDone(bool) {
      this.todoLists.forEach(list => {
        list.active = bool
      })
    },
    clearCompletedLists() {
      this.todoLists = this.todoLists.filter(item => item.active)
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