<template>
  <view class="add-todo-page">
    <view class="header">
      <text class="page-title">Add a new todo</text>
    </view>
    <view class="content">
      <input v-model="newTodo" placeholder="Enter your new todo"/>
      <button @tap="saveTodo">Save</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
    };
  },
  methods: {
    saveTodo() {
      let newText = this.newTodo.trim();
      uni.navigateTo({
        url: '../index/index',
        success: function (res) {
          if (newText !== '') {
            res.eventChannel.emit('saveTodo', newText)
            this.newTodo = '';
          }
        }
      })
    }
  },
};
</script>

<style>
.add-todo-page {
  padding: 20px;
}

.header {
  text-align: center;
  margin-bottom: 20px;
}

.page-title {
  font-size: 18px;
  font-weight: bold;
}

.content {
  display: flex;
  flex-direction: column;
}

input {
  margin-bottom: 10px;
  padding: 10px;
}

button {
  padding: 10px;
  background-color: #007BFF;
  color: #fff;
  border: none;
  cursor: pointer;
}
</style>