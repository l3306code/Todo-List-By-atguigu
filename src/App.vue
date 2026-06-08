<template>
  <div id="root">
  <div class="todo-container">
    <div class="todo-wrap">
      <HeaderVue ref="jlHeader"/>
      <List 
      ref="myList"
      :todos="todos"
      :checkTodo="checkTodo"
      :handle-delete="handleDelete"
      />
      <Footer 
        ref="myFooter"
        :todos="todos"
        :handleDeleteDone="handleDeleteDone"
      />
    </div>
  </div>
</div>
</template>

<script>
import HeaderVue from './components/Header.vue';
import Footer from './components/Footer.vue';
import List from './components/List.vue';


export default {
  name: 'App',
  components: {
    HeaderVue,
    Footer,
    List
  },
  data() {
    return {
      todos:JSON.parse(localStorage.getItem('todos')) || []
    };
  },
  methods: {
     //取消勾选或勾选todo
     checkTodo(id){
       this.todos.forEach(
        (todo) =>{
           if(todo.id === id)  todo.done = !todo.done
        })
     },
     //删除单个TODO
     handleDelete(id){
       this.todos = this.todos.filter((todo) => todo.id !== id)
     },
     //全选 全不选状态框
   /*   checkAllTodo(status){
       this.todos.forEach((todo) => todo.done = status)
     }, */
     //删除已完成任务
     handleDeleteDone(){
        this.todos = this.todos.filter((todo) => !todo.done)
     }

    },
    watch: {
       todos: {
        handler(newVal){
          localStorage.setItem('todos', JSON.stringify(newVal));
        },
        //需要开启深度监视才能看到localStorage todos数组内部对象的变化
        deep: true
       }
    },
    mounted() {
      const todos = localStorage.getItem('todos');
      if(todos) this.todos = JSON.parse(todos);

      this.$refs.myFooter.$on('checkAllTODO', (status) =>{
         this.todos.forEach((todo) => todo.done = status)
      })


      //添加待办事项
      this.$refs.jlHeader.$on('sendTodo', (todoObj) =>{
        this.todos.unshift(todoObj);
      })
    }
}
</script>

<style>
  /*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

</style>
