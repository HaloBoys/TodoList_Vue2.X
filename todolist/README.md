# 组件化编码流程

## 实现静态组件

组件要按照功能点拆分，命名不要与html元素冲突。

1. 抽取组件
2. 使用组件实现静态页面效果

## 展示动态数据

考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用：

一个组件在用：放在组件自身即可。
一些组件在用：放在他们共同的父组件上（状态提升）。

1. 数据的类型、名称是什么？
2. 数据保存在哪个组件？

## 交互

从绑定事件监听开始

# Todo 案例基本功能 V1.0

## 添加Todo功能

### 抽取组件

![20220917143359](https://raw.githubusercontent.com/HaloBoys/PicGoMyDevice/main/img/20220917143359.png)

### 数据传递

header 输入框中的value值获取的两种方式：

1. 通过事件对象 event 获取：`event.target.value`
2. 给输入框绑定 `v-model` 指令，数据双向绑定获取指定 `value`

兄弟间数据传递（原始方案）：

![20220917221654](https://raw.githubusercontent.com/HaloBoys/PicGoMyDevice/main/img/20220917221654.png)

## 勾选功能

监听 item 项的 checked 框是否被勾选，可以通过以下两种事件来监听：

1. click
2. change

实现思路:

1. 监听 item 项的 checked 勾选状态，获取被勾选 item 项的 id
2. 在 App 中定义修改 data 中 todo 的方法，并将该方法通过 props 传递给 item 组件，然后在 item 组件监听函数中调用该方法，并将 id 作为参数传递。

## 删除功能

实现思路: 

1. 给 item 项的删除按钮绑定事件，将被删除的 item 的 id 传入事件处理函数
2. 在 App 中定义删除按钮的业务逻辑函数，并将这个函数通过 props 传给 item，然后在 item 组件监听函数中调用该方法，并将 id 作为参数传递。

## 底部功能

### 已完成/全部 展示

已完成:

使用计算属性实现：计算todo列表中done状态为true的个数，有两种实现思路：

- forEach

```javascript
computed: {
 doneTotal() {
   let i = 0;
   this.todoArr.forEach((item) => {
     if (item.done == true) {
       i++;
     }
     console.log(item);
   });
   return i;
 },
},
```

- reduce

```javascript
doneTotal() {
  return this.todoArr.reduce((pre,current) => pre + (current.done ? 1 : 0),0)
},

```

全部:

直接使用 todolist数组 length 属性获取 （通过props 传给底部）

### footer区域展示与隐藏

当todo个数为0时，应该隐藏底部footer区域，实现思路：

使用 v-show 指令绑定 todolist数组的长度，当数组的长度为 0 时，v-show 为 false

### 全选/全不选

监听全选/全不选状态框，获取布尔值，然后将此状态直接批量给todolist数组中每一个todo的状态。

当已完成和全部相等的时候，则全选/全不选按钮为勾选状态。实现思路：

给状态框设置计算属性：`:checked="isChecked"`:

```javascript
computed: {
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
  // 依赖上面两个计算属性，当已完成和全部相等的时候，则底部全选按钮需要被勾选,注意此处加了一个逻辑与判断，如果不加，则会出现当已完成和全部都为0的时候，出现勾选的bug
  isAll() {
    return this.doneTotal === this.totalList && this.doneTotal > 0;
  },
},
```

### 清除已完成任务

使用数组过滤方法filter过滤todolist数组，返回todo中done为false的todo项，并覆盖原数组：

```javascript
// 清除已完成任务
cleanFinishedTodo() {
  this.todoArr = this.todoArr.filter((item) => item.done == false);
}
```

# TEMP

单文件组件中组件嵌套的问题：

直接在被嵌套组件的父组件中引入该组件，并使用。（注意引入路径的问题，路径是相对于父组件的路径。）

使用v-model时要切记：

1. v-model绑定的值不能是props传过来的值，因为props是不可以修改的！
2. props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但**不推荐这样做**。

数组相关的方法：

1. filter
2. reduce
3. forEach