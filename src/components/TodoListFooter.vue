<!-- todolist 底部组件 -->
<template>
  <div :class="{'todolist-footer': 'true', 'ishided': hided}">
    <span class="todolist-length">
      <strong>{{ todoListsLength }}</strong> items left
    </span>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      hided: true,
      todoListsLength: 0
    };
  },
  props: {
    todoLists: Array
  },
  watch: {
    todoLists(oldValue, newValue) {
      this.resetState()
    }
  },
  mounted() {
    this.resetState()
  },
  methods: {
    resetState() {
      this.todoLists.length === 0 ? this.hided = true : this.hided = false
      this.todoListsLength = this.todoLists.length
   }
  }
}
</script>
<style lang='scss' scoped>
.todolist-footer {
  position: relative;
  width: 570px;
  background: #fff;
  color: #777;
  padding: 10px 15px;
  height: 20px;
  .todolist-length {
    position: absolute;
    left: 15px;
    line-height: 20px;
  }
  &:before {
    content: '';
    position: absolute;
    right: 0;
    bottom: 0;
    left: 0;
    height: 50px;
    overflow: hidden;
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2), 0 8px 0 -3px #f6f6f6, 0 9px 1px -3px rgba(0, 0, 0, 0.2), 0 16px 0 -6px #f6f6f6, 0 17px 2px -6px rgba(0, 0, 0, 0.2);
  }
}
.ishided {
  display: none;
}
</style>