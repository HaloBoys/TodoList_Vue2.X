<template>
  <div>
    <li v-for="todoObj in todoArr" :key="todoObj.id">
      <label>
        <input
          type="checkbox"
          :checked="todoObj.done"
          @change="todoChangeHandler(todoObj.id)"
        />
        <span>{{ todoObj.value }}</span>
      </label>
      <button class="btn btn-danger" @click="todoDeleteHandler(todoObj.id)">
        删除
      </button>
    </li>
  </div>
</template>

<script>
export default {
  name: "MyItem",
  props: ["todoArr", "checkTodo", "deleteTodo"],
  methods: {
    todoChangeHandler(id) {
      this.checkTodo(id);
    },
    todoDeleteHandler(id) {
      if (confirm("确定删除该任务吗？")) {
        this.deleteTodo(id);
      }
    },
  },
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
li:hover {
  background-color: #ccc;
}
li:hover button {
  display: block;
}
</style>
