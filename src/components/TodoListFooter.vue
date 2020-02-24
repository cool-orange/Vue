<!-- todolist 底部组件 -->
<template>
  <div :class="{'todolist-footer': 'true', 'ishided': hided}">
    <span class="todolist-length">
      <strong>{{ activeListsLength }}</strong> items left
    </span>
    <div class="todolist-filter">
      <div :class="{'btn-all': 'true', 'selected': checkedBtn == 'all' }" @click="filtertodoList('all')">All</div>
      <div :class="{'btn-active': 'true', 'selected': checkedBtn == 'active' }" @click="filtertodoList('active')">Active</div>
      <div :class="{'btn-completed': 'true', 'selected': checkedBtn == 'completed'}" @click="filtertodoList('completed')">Completed</div>
    </div>
    <div class="todolist-clear" @click="clearCompleted">Clear completed</div>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      hided: true,
    };
  },
  props: {
    todoLists: Array,
    activeListsLength: Number,
    checkedBtn: String
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
   },
   filtertodoList(key) {
     this.$emit('filter', key)
   },
   clearCompleted() {
     this.$emit('clear')
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
    top: 11px;
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
  .todolist-filter {
    position: relative;
    width: 100%;
    height: auto;
    .btn-all, .btn-active, .btn-completed {
      display: inline;
      padding: 3px 5px;
      margin: 0 3px;
      box-sizing: border-box;
      //设置悬浮时不会产生抖动
      border: 1px solid transparent;
    }
    .selected {
      border: 1px solid rgba(175, 47, 47, 0.2);;
      border-radius: 4px;
    }
    @each $btn in all, active, completed {
      .btn-#{$btn} {
        &:hover {
          border: 1px solid rgba(175, 47, 47, 0.2);;
          border-radius: 4px;
        }
      }
    }

  }
  .todolist-clear {
    position: absolute;
    top: 11px;
    right: 15px;
    &:hover {
      text-decoration: underline;
    }
  }
}
.ishided {
  display: none;
}
</style>