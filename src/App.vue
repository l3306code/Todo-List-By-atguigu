<template>
  <div id="root">
  <div class="todo-container">
    <div class="todo-wrap">
      <HeaderVue ref="jlHeader"/>
      <List 
      ref="myList"
      :todos="todos"
      />
      <Footer 
        ref="myFooter"
        :todos="todos"
        @handleDeleteDone="handleDeleteDone"
      />
    </div>
  </div>
</div>
</template>

<script>
import HeaderVue from './components/Header.vue';
import Footer from './components/Footer.vue';
import List from './components/List.vue';
import PubSub, { publish }  from 'pubsub-js';


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

      //全局事件总线，接收组件数据

      //取消勾选或勾选todo

      //版本一: 全局事件总线 
      /* this.$bus.$on('checkTodo', (id) =>{
          this.todos.forEach(
        (todo) =>{
           if(todo.id === id)  todo.done = !todo.done
        })
      }) */

      //版本二: 使用pubsub
      this.ctPubId = PubSub.subscribe('checkTodo', (_,id) =>{
         this.todos.forEach(
        (todo) =>{
           if(todo.id === id)  todo.done = !todo.done
        })
      })
   
      // 删除单个todo

      //版本一: 全局事件总线
      /* this.$bus.$on('handleDelete', (id) => {
         this.todos = this.todos.filter((todo) => todo.id !== id)
      })
      */

      //版本2: 使用pubsub
      this.hdPubId = PubSub.subscribe('handleDelete', (_, id) =>{
        console.log('我收到了id', id);
        this.todos = this.todos.filter((todo) => todo.id !== id)
      })


      this.$bus.$on('updateTodo', (id, newTodoTitle) =>{
        
        this.todos.forEach(
          (todo) =>{
            if(todo.id === id) todo.title = newTodoTitle
          }
        )
      })
       
    },
    beforeDestroy(){
      /* this.$bus.$off('checkTodo')
      this.$bus.$off('handleDelete') */

      this.$bus.$off('updateTodo')
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

.btn-edit {
  color: #fff;
  background-color: skyblue;
  border: 1px solid rgb(113, 166, 187);
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
