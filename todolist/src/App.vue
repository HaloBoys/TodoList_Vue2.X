<template>
  <div id="app">
    <div id="root">
      <div class="todo-container">
        <div class="todo-wrap">
          <MyHeader :addTodo="addTodo" />
          <MyList
            :todoArr="todoArr"
            :checkTodo="checkTodo"
            :deleteTodo="deleteTodo"
          />
          <MyFooter
            :todoArr="todoArr"
            :checkedTodo="checkedTodo"
            :cleanFinishedTodo="cleanFinishedTodo"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import MyHeader from "./components/MyHeader.vue";
import MyList from "./components/MyList.vue";
import MyFooter from "./components/MyFooter.vue";

export default {
  name: "App",
  data() {
    return {
      todoArr: [
        { id: "001", value: "001", done: false },
        { id: "002", value: "002", done: false },
        { id: "003", value: "003", done: true },
      ],
    };
  },
  methods: {
    // 添加Todo功能
    addTodo(todo) {
      this.todoArr.unshift(todo);
    },
    // 勾选功能
    checkTodo(id) {
      this.todoArr.forEach((item) => {
        if (item.id === id) {
          item.done = !item.done;
        }
      });
    },
    // 删除功能
    deleteTodo(id) {
      this.todoArr = this.todoArr.filter((item) => item.id !== id);
    },
    // 全选/全不选功能
    checkedTodo(isCheck) {
      this.todoArr.forEach((item) => (item.done = isCheck));
    },
    // 清除已完成任务
    cleanFinishedTodo() {
      this.todoArr = this.todoArr.filter((item) => item.done == false);
    },
  },
  components: {
    MyHeader,
    MyList,
    MyFooter,
  },
};
</script>

<style scoped>
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
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
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
