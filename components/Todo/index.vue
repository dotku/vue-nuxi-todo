<script setup>
import { Switch, SwitchGroup, SwitchLabel } from '@headlessui/vue'
</script>

<script>
let id = 0;
export default {
  data() {
    return {
      newTodo: '',
      hideCompleted: false,
      todos: [
        { id: id++, text: "Problem Definition", done: false },
        { id: id++, text: "Data Collection", done: false },
        { id: id++, text: "Data Analysis and Exploration", done: false },
        { id: id++, text: "Model Building", done: false },
        { id: id++, text: "Model Deployment", done: false },
        { id: id++, text: "Model Monitoring and Maintenance", done: false },
      ]
    }
  },
  methods: {
    addTodo() {
      this.todos.push({ id: id++, text: this.newTodo, done: false })
      this.newTodo = ''
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    }
  },
  computed: {
    filteredTodos() {
      return this.hideCompleted ? this.todos.filter(item => !item.done) : this.todos;
    }
  }
}
</script>

<template>
  <div class="mx-auto max-w-7xl py-6 px-4 sm:px-6 lg:px-8">
    <form @submit.prevent="addTodo" class="relative sm:max-w-md m-auto mb-3 space-x-6 text-sm">
      <div class="flex justify-center mb-2">
        <input type="text" name="todo" id="todo"
          class="shadow-sm block w-full rounded-md border-0 px-3 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          placeholder="What do you want todo?" v-model="newTodo" />
        <button
          class="ml-3 bg-slate-900 hover:bg-slate-700 focus:outline-none focus:ring-2 focus:ring-slate-400 focus:ring-offset-2 focus:ring-offset-slate-50 text-white font-semibold h-12 px-6 rounded-lg w-full flex items-center justify-center sm:w-auto dark:bg-sky-500 dark:highlight-white/20 dark:hover:bg-sky-400">Add</button>
      </div>
      <SwitchGroup as="div" class="flex">
        <div class="flex h-6 items-center">
          <Switch v-model="hideCompleted"
            :class="[hideCompleted ? 'bg-indigo-600' : 'bg-gray-200', 'flex w-8 flex-none cursor-pointer rounded-full p-px ring-1 ring-inset ring-gray-900/5 transition-colors duration-200 ease-in-out focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600']">
            <span aria-hidden="true"
              :class="[hideCompleted ? 'translate-x-3.5' : 'translate-x-0', 'h-4 w-4 transform rounded-full bg-white shadow-sm ring-1 ring-gray-900/5 transition duration-200 ease-in-out']" />
          </Switch>
        </div>
        <SwitchLabel class="text-sm leading-6 text-gray-600 ml-3">
          {{ !hideCompleted ? 'Show all' : 'Hide completed' }}
        </SwitchLabel>
      </SwitchGroup>
    </form>


    <ul class="flex flex-col w-full sm:max-w-md m-auto">
      <li class="w-full bg-gray-50 dark:bg-white/5 p-3 rounded-md mb-1" v-for="todo in filteredTodos" :key="todo.id">
        <div class="flex justify-between">
          <span>
            <input type="checkbox" v-model="todo.done">
            <span class="ml-3" :class="{ done: todo.done }">{{ todo.text }}</span>
          </span>
          <button @click="removeTodo(todo)">X</button>
        </div>
      </li>
    </ul>

  </div>
</template>
<style>
.done {
  text-decoration: line-through;
}
</style>
