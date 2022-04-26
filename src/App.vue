<script>
import { TrashIcon, PlusSmIcon } from '@heroicons/vue/solid';
import TodoComponent from './components/TodoComponent.vue'

export default {
  name: 'App',
  components: { 
    PlusSmIcon,
    TrashIcon ,
    TodoComponent
  },
  data() {
    return {
      todos: [],
      todo: "",
      editTodo: "",
      editTodoIdx: ""
    }
  },
  methods: {
    removeTodo(index) {
      this.todos.splice(index, 1)
      this.updateTodos();
    },
    addTodo() {
      if(this.todo.length < 1) return;
      this.todos.push(this.todo);
      this.todo = "";
      this.updateTodos();
    },
    updateTodos() {
      console.log("updateTodos");
      window.localStorage.setItem("todos", JSON.stringify(this.todos));
    },
    toggleEditTodoFunction(index) {
      this.editTodo = this.todos[index];
      this.editTodoIdx = index;
      console.log(this.$refs.todos);
    },
    updateTodo(index, e) {
      this.todos[index] = e.target.value; 
      this.updateTodos()
    },
    keyDown(e) {
      if(e.key === "Enter") {
        this.addTodo();
      }
    }
  },
  mounted() {
    const todos = window.localStorage.getItem("todos");
    if(!todos) return;
    this.todos = JSON.parse(todos);
  }
}
</script>

<template>
  <div class="flex flex-col items-center m-10">
    <h1 class="text-3xl">Todos</h1>
    <div class="flex gap-2 my-4">
      <div>
      <label htmlFor="text" class="sr-only">
        Todo
      </label>
      <input
        type="text"
        name="text"
        id="text"
        class="shadow-sm focus:ring-green-500 focus:border-green-500 block w-full sm:text-sm border-stone-700 rounded-md bg-stone-800"
        placeholder="Todo.."
        :value="todo"
        @input="event => todo = event.target.value"
        @keydown="keyDown"
      />
    </div>
    <div class="flex items-center">
      <button
        type="button"
        class="inline-flex items-center p-1 border border-transparent rounded-full shadow-sm text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500"
        @click="addTodo()"
      >
        <PlusSmIcon class="h-5 w-5" aria-hidden="true" />
      </button>
    </div>
    </div>
    <div class="max-w-sm w-full mx-5 mt-4">
      <todo-component 
        v-for="(todo, index) in todos" 
        :key="todo" 
        :todo="todo" 
        :index="index"
        @update-text="(e) => {console.log(e)}"
        @remove="removeTodo(index)"
        @edit-todo="toggleEditTodoFunction(index)"
      />
    </div>
  </div>
</template>

<style>
@import "./assets/base.css";
</style>