<template>
	<div id="app">
		<todo-header></todo-header>
		<todo-input v-on:addTodoItem="addOneItem"></todo-input>
		<todo-list
			v-bind:propsdata="todoItems"
			v-on:removeItem="removeOneItem"
			v-on:toggleItem="toggleOneItem"
		></todo-list>
		<todo-footer></todo-footer>
	</div>
</template>

<script>
import TodoHeader from './components/TodoHeader';
import TodoInput from './components/TodoInput';
import TodoList from './components/TodoList';
import TodoFooter from './components/TodoFooter';

export default {
	data: function() {
		return {
			todoItems: [],
		};
	},
	methods: {
		addOneItem: function(todoItem) {
			var obj = { item: todoItem, completed: false };
			localStorage.setItem(todoItem, JSON.stringify(obj));
			this.todoItems.push(obj);
		},
		removeOneItem: function(todoItem, index) {
			localStorage.removeItem(todoItem.item);
			this.todoItems.splice(index, 1);
		},
		toggleOneItem: function(todoItem, index) {
			console.log(todoItem, index);
			todoItem.completed = !todoItem.completed;
			localStorage.removeItem(todoItem.item);
			localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
		},
	},
	created: function() {
		if (localStorage.length > 0) {
			for (var i = 0; i < localStorage.length; i++) {
				if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
					this.todoItems.push(
						JSON.parse(localStorage.getItem(localStorage.key(i))),
					);
				}
			}
		}
	},
	components: {
		TodoHeader,
		TodoInput,
		TodoList,
		TodoFooter,
	},
};
</script>

<style>
body {
	text-align: center;
	background-color: #f6f6f6;
}
input {
	border-style: groove;
	width: 200px;
}
button {
	border-style: groove;
}
.shadow {
	box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
