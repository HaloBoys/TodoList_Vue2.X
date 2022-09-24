<template>
  <div class="todo-header">
    <input
      type="text"
      placeholder="请输入你的任务名称，按回车键确认"
      @keyup.enter="enterHandle"
      v-model="typeValue"
    />
  </div>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  name: "MyHeader",
  // props: ["addTodo"],
  data() {
    return {
      typeValue: "",
    };
  },
  methods: {
    // 1. 通过事件对象获取文本框值
    // enterHandle(e) {
    //   this.addTodo({id:nanoid(),value:e.target.value,done:false})
    // },

    // 2. 通过 v-model 指令获取文本框值
    enterHandle() {
      if (!this.typeValue.trim()) {
        return alert("输入的数据不能为空！");
      }
      // this.addTodo({id:nanoid(),value:this.typeValue,done:false})
      this.$emit("addTodo", {
        id: nanoid(),
        value: this.typeValue,
        done: false,
      });

      // 清空输入框
      this.typeValue = "";
    },
  },
};
</script>

<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}

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
