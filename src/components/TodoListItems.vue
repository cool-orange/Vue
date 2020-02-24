<!-- todolist主体部分 -->
<template>
  <div class='todolist-main'>
    <div :class="{'select-all': 'true', 'select-hided': hided }" @click="completeAll"></div>
    <div :class="'todolist-wrap'" v-for="(todoList, index) in todoLists" :key="index + todoLists.length">
      <TodoListItem 
      :todoList="todoList"
      :index="index"
      v-if="todoList.active == true && checkedBtn == 'active' || todoList.active == false && checkedBtn == 'completed' || checkedBtn == 'all'"
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
  data() {
    return {
      alldone: false
    }
  },
  props: {
    todoLists: Array,
    checkedBtn: String
  },
  computed: {
    hided() {
      return this.todoLists && this.todoLists.length > 0 ? false : true
    }
  },
  methods: {
    completeAll() {
      this.$emit('completedone', this.alldone)
      this.alldone = !this.alldone
    }
  }
}
</script>
<style lang='scss' scoped>
.todolist-main {
  position: relative;
  width: 100%;
  .select-all {
    position: absolute;
    width: 60px;
    height: 34px;
    top: -48px;
    left: -7px;
    -webkit-transform: rotate(90deg);
    transform: rotate(90deg);
    &:before {
      position: relative;
      content: '❯';
      font-size: 22px;
      color: #e6e6e6;
      padding: 10px 27px 10px 27px;
      z-index: 1;
    }
  }
  .todolist-wrap {
    position: relative;
  }
  .select-hided {
    opacity: 0;
  }
}
</style>