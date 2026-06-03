<template>
  <li>
    <label>
      <!-- 不太推荐的写法，因为修改的props，但vue2未检测到 -->
      <input type="checkbox" v-model="todoObj.done" />
      <span>{{ todoObj.title }}</span>
    </label>
    <button class="btn btn-danger" style="display:none">删除</button>
  </li>
</template>

<script>
export default {
  name: "Itemvue",
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
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
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

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
</style>