<!-- todolist输入框组件 -->
<template>
<div class='todolist-input'>
  <input autofocus="autofocus" placeholder="What needs to be done?" @keydown="entertoadd" v-model="content">
</div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      content: ''
    };
  },
  methods: {
    entertoadd(e) {
      if(this.content === '') {
        return
      }
      
      if(e.key === 'Enter') {
        this.$emit('entertoadd', this.content)
        this.content = ''
      }
    }
  }
}
</script>
<style lang='scss' scoped>
.todolist-input {
  width: 100%;
  margin: 0 auto;
  input {
    width: 100%;
    padding: 16px 16px 16px 60px;
    font-size: 24px;
    border: none;
    -webkit-box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
    box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
    box-sizing: border-box;
    outline: none;
  }
  ::-webkit-input-placeholder {
    font-style: italic;
    color: #e6e6e6;
  }
  //Firefox 4 to 18
  ::-moz-placeholder {
    font-style: italic;
    color: #e6e6e6;
  }
  //Firefox 19+
  :-moz-placeholder {
    font-style: italic;
    color: #e6e6e6;
  }
  //IE 10+ 
  :-ms-input-placeholder {
    font-style: italic;
    color: #e6e6e6;
  }
}
</style>