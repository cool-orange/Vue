<!-- todolist 每一个子项目-->
<template>
  <div :class="{todolist: 'true', editing: isEdited}">
    <input type="checkbox" class="toogle" @click="checkItem">
    <label 
    :class="{text: 'true', checked: isChecked}"
    @dblclick="editItem(index)"
    >{{ todoList }}</label>
    <button class="destroy" @click="deleteItem(index)"></button>
    <input ref="editInput" type="text" class="edit" v-model="inputContent"  
    @keydown="confirmItem"
    @blur="cancelEdit"
    spellcheck ="false">
  </div>
</template>

<script>
export default {
  props: {
    todoList: String,
    index: Number
  },
  data() {
    return {
      isChecked: false,
      isEdited: false,
      inputContent: ''
    };
  },
  mounted() {
    this.inputContent = this.todoList
  },
  watch: {
    inputContent(newValue, oldValue) {
      this.$parent.$parent.editItem(this.index, newValue)
    }
  },
  methods: {
    checkItem() {
      this.isChecked = !this.isChecked
    },
    deleteItem(index) {
      this.$parent.$parent.deleteItem(index)
    },
    editItem(index) {
      this.isEdited = true
      this.$nextTick(()=>{
        this.$refs.editInput.focus()
      })
    },
    cancelEdit() {
      this.isEdited = false
    },
    confirmItem(e) {
      if(e.key === 'Enter') {
        this.isEdited = false
      }
    }
  }
}
</script>
<style lang='scss' scoped>
.todolist {
    position: relative;
    width: 100%;
    font-size: 24px;
    box-sizing: border-box;
    border-bottom: 1px solid #ededed;
    background: #fff;
    &:hover .destroy {
      display: block;
    }
    .toogle {
      position: absolute;
      margin: auto 0;
      top: 0;
      bottom: 0;
      width: 40px;
      height: 40px;
      left: 0;
      opacity: 0;
      -webkit-appearance: none;
      appearance: none;
    }
    .text {
      display: block;
      width: 100%;
      text-align: left;
      padding: 15px 15px 15px 60px;
      box-sizing: border-box;
      background-image: url('data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23ededed%22%20stroke-width%3D%223%22/%3E%3C/svg%3E');
      background-repeat: no-repeat;
      background-position: center left;
    }
    .checked {
      background-image: url('data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23bddad5%22%20stroke-width%3D%223%22/%3E%3Cpath%20fill%3D%22%235dc2af%22%20d%3D%22M72%2025L42%2071%2027%2056l-4%204%2020%2020%2034-52z%22/%3E%3C/svg%3E');
      background-repeat: no-repeat;
      background-position: center left;
      color: #d9d9d9;
      text-decoration: line-through;
    }
    .destroy {
      display: none;
      position: absolute;
      right: 0;
      height: 40px;
      width: 40px;
      top: 0;
      bottom: 0;
      margin: auto 0;
      color: #cc9a9a;
      font-size: 30px;
      background: none;
      vertical-align: middle;
      font-family: inherit;
      font-weight: inherit;
      border: 0;
      outline: none;
      &:after {
        content: '×';
      }
    }
    .edit {
      width: 100%;
      position: relative;
      display: none;
      font-size: 24px;
      font-family: inherit;
      font-weight: inherit;
      border: 0;
      line-height: 1.4em;
      color: inherit;
      padding: 6px;
      border: 1px solid #999;
      box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
      box-sizing: border-box;
      outline: none;
    }
    .editing {
      .edit {
        display: block;
        width: 557px;
        padding: 12px 16px;
        margin: 0 0 0 43px;
        &:last-child {
          margin-bottom: -1px;
        }
      }
      .toggle {
        display: none;
      }
      .destroy {
        display: none;
      }
      .text {
        display: none;
      }
    }
  }
</style>