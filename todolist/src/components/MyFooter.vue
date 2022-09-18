<template>
  <div class="todo-footer" v-show="totalList">
    <label>
      <input type="checkbox" @change="isCheckAll" :checked="isAll" />
    </label>
    <span>
      <span>已完成 {{ doneTotal }}</span> / 全部 {{ totalList }}</span
    >
    <button class="btn btn-danger" @click="cleanFinishedTodo">
      清除已完成任务
    </button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  props: ["todoArr", "checkedTodo", "cleanFinishedTodo"],
  computed: {
    // doneTotal() {
    //   let i = 0;
    //   this.todoArr.forEach((item) => {
    //     if (item.done == true) {
    //       i++;
    //     }
    //     console.log(item);
    //   });
    //   return i;
    // },
    // 已完成
    doneTotal() {
      return this.todoArr.reduce(
        (pre, current) => pre + (current.done ? 1 : 0),
        0
      );
    },
    // 全部
    totalList() {
      return this.todoArr.length;
    },
    // 依赖上面两个计算属性，当已完成和全部相等的时候，则底部全选按钮需要被勾选
    // 注意此处加了一个逻辑与判断，如果不加，则会出现当已完成和全部都为0的时候，出现勾选的bug
    isAll() {
      return this.doneTotal === this.totalList && this.doneTotal > 0;
    },
  },
  methods: { 
    isCheckAll(event) {
      this.checkedTodo(event.target.checked);
    },
  },
};
</script>

<style scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
