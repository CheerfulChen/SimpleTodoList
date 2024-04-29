<template>
	<uni-nav-bar :fixed="true" shadow background-color="#e3f9de" status-bar right-icon="plus" title="Todo List"
		color="black" style="top: 0;" @clickRight="jump" />
	<view class="container">
		<view class="header">
			<input class="add" v-if="todos.length == 0" v-model="newTodo" @confirm="addTodo"
				placeholder="Add a new todo" />
		</view>
		<view class="todo-list">
			<view v-for="(todo, index) in todos" :key="index" class="todo-item">
				<text :class="{ completed: todo.completed }">{{ todo.text }}</text>
				<button @tap="toggleTodo(index)">Toggle</button>
				<button @tap="deleteTodo(index)">Delete</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				newTodo: '',
				todos: [],
				saveTodoListenerAdded: false,
			};
		},
		mounted() {
			this.todos = uni.getStorageSync('todos') || [];
		},
		onShow() {
			this.todos = uni.getStorageSync('todos') || [];
		},
		methods: {
			addTodo() {
				const newText = this.newTodo.trim();
				if (newText !== '') {
					this.todos.push({
						text: newText,
						completed: false
					});
					this.newTodo = '';
					this.saveTodos();
				}
			},
			jump() {
				uni.navigateTo({
					url: '/pages/AddTodo/AddTodo',
				});
			},

			toggleTodo(index) {
				this.todos[index].completed = !this.todos[index].completed;
				this.saveTodos();
			},
			deleteTodo(index) {
				this.todos.splice(index, 1);
				this.saveTodos();
			},
			saveTodos() {
				uni.setStorageSync('todos', this.todos);
			},
		},
		onLoad: function() {
			const eventChannel = this.getOpenerEventChannel()
			eventChannel.on('saveTodo', (res) => {
				let trimmedText = res.trim();
				if (trimmedText !== '') {
					trimmedText = res.trim();
					this.todos.push({
						text: trimmedText,
						completed: false
					});
					this.saveTodos();
				}
			})
		}
	};
</script>

<style>
	.container {
		position: fixed;
		top: 44px;
		//display: flex;
		//flex-direction: column;
		//align-items: center;
		//justify-content: center;
		//top: 44px;
		//height: 100vh;
	}

	.header {
		margin-bottom: 20px;
	}

	.add {
		//position: fixed;
		//top: 50%;
		//left: 50%;
		//justify-content: center;
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
		width: 100vw;
		margin: 0;
		text-align: center;
	}

	.todo-list {
		position: fixed;
		width: 100%;
	}

	.todo-item {
		display: flex;
		justify-content: space-between;
		margin: 20px;
		padding: 10px;
		border-bottom: 1px solid #eee;
	}

	.completed {
		text-decoration: line-through;
		color: #868e96;
	}

	.Todo {
		display: flex;
		justify-content: center;
		margin: 15px;
		font-size: 19px;

	}
</style>