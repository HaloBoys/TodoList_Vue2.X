<template>
  <div>
    <li v-for="(todoObj, index) in todoArr" :key="todoObj.id">
      <label>
        <input
          type="checkbox"
          :checked="todoObj.done"
          @change="todoChangeHandler(todoObj.id)"
        />
        <span v-show="!todoObj.isEdit">{{ todoObj.value }}</span>
        <input
          ref="editinput"
          type="text"
          :value="todoObj.value"
          v-show="todoObj.isEdit"
          @blur="todoEditBlur(todoObj, $event)"
          @keyup.enter="todoEditBlur(todoObj, $event)"
        />
      </label>
      <button class="btn btn-danger" @click="todoDeleteHandler(todoObj.id)">
        删除
      </button>
      <button
        v-show="!todoObj.isEdit"
        class="btn btn-primary"
        @click="todoEditHandler(todoObj, index)"
      >
        编辑
      </button>
    </li>
  </div>
</template>

<script>
import pubsub from "pubsub-js";

export default {
  name: "MyItem",
  // props: ["todoArr", "checkTodo", "deleteTodo"],
  props: ["todoArr"],
  methods: {
    todoChangeHandler(id) {
      // this.checkTodo(id);
      // this.$bus.$emit("checkTodo", id);
      pubsub.publish("checkTodo", id);
    },
    todoDeleteHandler(id) {
      if (confirm("确定删除该任务吗？")) {
        // this.deleteTodo(id);
        // this.$bus.$emit("deleteTodo", id);
        pubsub.publish("deleteTodo", id);
      }
    },
    todoEditHandler(todoObj, index) {
      this.$set(todoObj, "isEdit", true);
      this.$nextTick(() => {
        console.log(index);
        this.$refs.editinput[index].focus();
      });
    },
    todoEditBlur(todoObj, event) {
      // 失去焦点文本框变文本
      todoObj.isEdit = false;
      if (!event.target.value.trim()) return alert("内容不能为空！");
      this.$bus.$emit("updateTodo", todoObj.id, event.target.value);
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
