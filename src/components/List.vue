<template>
  <ul class="todo-main">
    <ItemVue v-for="todoObj in todos" :key="todoObj.id" 
    :todoObj="todoObj"
    :handleDelete="handleDelete"/>
  </ul>
</template>

<script>
import ItemVue from './Item.vue';

export default {
  name: "List",
  components: {
    ItemVue
  },
  props: ['todos', 'handleDelete'],
  data() {
    return {
      

    };
  },
  watch: {

  },
  computed: {

  },
  methods: {

  },
  created() { },
  mounted() { }
};
</script>
<style scoped>
/*main*/
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>