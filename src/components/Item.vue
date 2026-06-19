<template>
  <li>
    <label>
      <input type="checkbox" :checked="todoObj.done" @change="handleCheck(todoObj.id)"/>
      <span v-show="!todoObj.isEdit">{{ todoObj.title }}</span>
      <input 
        v-show="todoObj.isEdit" 
        type="text" 
        :value="todoObj.title" 
        @blur="handleBlur(todoObj, $event)"
        ref="inputTitle"
      >
    </label>
    <button class="btn btn-danger" @click="deleteTodo(todoObj.id)" >删除</button>
    <button class="btn btn-edit"  @click="handleEdit(todoObj)">编辑</button>
  </li>
</template>

<script>
import PubSub from 'pubsub-js';
export default {
  name: "ItemVue",
  components: {

  },
  //父传子核心操作
  props: ['todoObj'],
  data() {
    return {
    };
  },
  watch: {

  },
  computed: {

  },
  methods: {
    handleCheck(id){
        console.log(id);       
      
        // this.$bus.$emit('checkTodo', id)
        PubSub.publish('checkTodo', id)
    },
    deleteTodo(id){
        if(confirm('确定删除吗？')){

          // this.$bus.$emit('handleDelete', id)
          console.log("删除执行", id);
          PubSub.publish('handleDelete', id) 
        }
    },
    // 编辑
    handleEdit(todoObj){
        if(todoObj.hasOwnProperty('isEdit')){
          todoObj.isEdit = true
        }else {
          this.$set(todoObj, 'isEdit', true)
        } 
        
        this.$nextTick( ()=>{
           this.$refs.inputTitle.focus()
        })
    },
    //失去焦点回调(真正执行修改逻辑)
    handleBlur(todoObj, e){
      todoObj.isEdit = false
      if(!e.target.value.trim()){
        confirm('待办事项不得为空！！！')

        return
      }
      this.$bus.$emit('updateTodo', todoObj.id, e.target.value)
    }
  },
  created() { },
  mounted() { }
};
</script>
<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;

  display: flex;
  justify-content: space-between;
  align-items: center;
}


.btns {
  display: flex;
  gap: 8px;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

.btns button {
  display: none;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover{
  background: #ddd;

}

li:hover .btns button {
  display: block;
}
</style>