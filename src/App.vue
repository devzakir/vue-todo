<template>
  <div id="app">
    <section class="todoapp">
			<header class="header">
				<h1>todos</h1>
				<input class="new-todo" autofocus v-model="todo" @keyup.enter="addTodo" placeholder="What needs to be done?">
			</header>
			<section class="main">
				<input id="toggle-all" class="toggle-all" type="checkbox">
				<label v-show="this.todos.length" for="toggle-all">Mark all as complete</label>
				<ul class="todo-list" v-show="this.todos.length">
					<li v-for="todo in filterTodos" v-bind:key="todo.id" :class="{ completed: todo.completed, editing: todo == editing }">
						<div class="view">
							<input class="toggle" v-model="todo.completed" type="checkbox" checked>
							<label @dblclick="editTodo(todo)">{{ todo.title }}</label>
							<button @click="deleteTodo(todo)" class="destroy"></button>
						</div>	
						<input class="edit" v-model="todo.title" v-on:blur="updateTodo(todo)" @keyup.enter="updateTodo(todo)">
					</li>
				</ul>
			</section>
			<footer v-show="this.todos.length" class="footer">
				<span class="todo-count"><strong v-text="activeTodos"></strong> item left</span>
				<ul class="filters">
					<li>
						<a href="#all" :class="{selected: tab == 'all' }" @click="checkHash('all')">All</a>
					</li>
					<li>
						<a href="#active" :class="{selected: tab == 'active' }" @click="checkHash('active')">Active</a>
					</li>
					<li>
						<a href="#completed" :class="{selected: tab == 'completed' }" @click="checkHash('completed')">Completed</a>
					</li>
				</ul>
				<button v-if="todos.length > activeTodos" @click="clearTodo" class="clear-completed">Clear completed</button>
			</footer>
		</section>
		<footer class="info">
			<p>Double-click to edit a todo</p>
			<p>Created by <a href="http://https://zakirhossen.com">Zakir Hossen</a></p>
			<p>Part of <a href="http://todomvc.com">TodoMVC</a></p>
		</footer>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data () {
      return {
		  todos: [
			  {
				  id: 1,
				  title: 'Taste JavaScript.',
				  completed: 0,
			  },
			  {
				  id: 2,
				  title: 'Buy a unicorn.',
				  completed: 0,
			  },
		  ],
		  todo: '',
		  editing: null,
		  tab: 'all',
	  }
	},
	methods: {
		addTodo(){
			this.todos.push({
				id: this.todos.length + 1,
				title: this.todo,
				completed: 0,
			});

			this.todo = '';
		},

		editTodo: function(todo) {
			this.editing = todo;			
		},

		updateTodo(todo){
			this.editing = null;
			todo.title = todo.title;
		},

		deleteTodo(todo){
			var index = this.todos.indexOf(todo);
			this.todos.splice(index, 1);
		},

		clearTodo(){
			this.todos = this.todos.filter( todo => todo.completed == 0);
		}, 

		checkHash(setTab){
			if(setTab == undefined){
				this.tab = 'all';
				window.location.hash = '';
			}else {
				this.tab = setTab;
			}
		}
	}, 
	mounted(){
		this.checkHash();
	},
	computed: {
		activeTodos(){
			return this.todos.filter( todo => todo.completed == 0).length;
		},
		filterTodos(){
			if(this.tab == 'all'){
				return this.todos;
			}else if(this.tab == 'active'){
				return this.todos.filter( todo => todo.completed == 0);
			}else if(this.tab == 'completed'){
				return this.todos.filter( todo => todo.completed == 1);
			}
		},
	}
  }
</script>

<style>
  @import url('../node_modules/todomvc-common/base.css');
  @import url('../node_modules/todomvc-app-css/index.css');
  .edit-todo {
	word-break: break-all;
	padding: 15px 15px 15px 60px;
	display: block;
	width: 100%;
	box-sizing: border-box;
	line-height: 1.2;
	transition: color 0.4s;
	font-weight: 400;
	color: #4d4d4d;
	font-size: 24px;
	border: 0;
	box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
  }
  .todoapp h1 {
	font-size: 100px;
	top: -155px;
	color: rgba(175, 47, 47, 0.15);
  }
</style>
