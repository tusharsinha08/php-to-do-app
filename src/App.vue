<template>
  <div class="app">
    <h1 class="header">To Do App</h1>
    <TodoInput @add-todo="addTodo" />

    <!-- Display the number of completed tasks -->
    <p>Completed tasks: {{ completedTodosCount }} / {{ todos.length }}</p>

    <transition-group name="fade" tag="ul" class="todo-list">
      <li v-for="(todo, index) in todos" :key="index" :class="{'todo-done': todo.done}" class="todo-item">
        <div v-if="editingIndex !== index" @dblclick="editTodo(index)">
          <span>{{ todo.text }}</span>
        </div>
        <div v-else>
          <input type="text" v-model="todos[index].text" @keyup.enter="stopEditing" @blur="stopEditing" />
        </div>
        <div class="actions">
          <button id="done" @click="markAsDone(index)" :disabled="todo.done">
            {{ todo.done ? 'Done' : 'Mark as Done' }}
          </button>
          <button id="delete" @click="deleteTodo(index)">Delete</button>
        </div>
      </li>
    </transition-group>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import TodoInput from './components/TodoInput.vue'

const todos = ref([])
const editingIndex = ref(null)

const addTodo = (todoText) => {
  todos.value.push({ text: todoText, done: false })
}

const markAsDone = (index) => {
  todos.value[index].done = true
}

const deleteTodo = (index) => {
  if (todos.value[index].done) {
    todos.value.splice(index, 1)
  } else {
    alert("Please mark the task as done before deleting it!")
  }
}

const editTodo = (index) => {
  editingIndex.value = index
}

const stopEditing = () => {
  editingIndex.value = null
}

const completedTodosCount = computed(() => {
  return todos.value.filter(todo => todo.done).length
})
</script>

<style>
/* Global styles */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

body {
  font-family: 'Poppins', sans-serif;
  background-color: #1a1a1a; /* Dark background for the entire app */
  color: #f0f0f0; /* Light color for contrast */
}

.header {
  font-size: 3rem;
  color: #00d1d1; /* Bright teal for header */
  font-weight: 600;
}

.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 4rem;
  text-align: center;
  background: #262626; /* Dark background for the app container */
  border-radius: 15px;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.5); /* Deeper shadow for depth */
}

h1 {
  color: #f0f0f0;
}

p {
  margin: 1rem 0;
  font-weight: bold;
  color: #cccccc; /* Lighter color for the completed tasks text */
}

.todo-list {
  list-style: none;
  padding: 0.5rem;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  background: #333333; /* Dark gray background for each todo item */
  margin: 10px 0;
  padding: 15px 20px; /* Slightly larger padding for more space */
  border-radius: 12px;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.3); /* Slight shadow for lift */
  transition: transform 0.3s ease;
}

.todo-item:hover {
  transform: scale(1.03); /* Larger scale effect on hover */
}

.todo-done span {
  text-decoration: line-through;
  color: #777; /* Dim color for completed tasks */
}

.actions {
  display: flex;
  gap: 10px;
}

input {
  padding: 8px;
  border: 1px solid #555; /* Darker border for input */
  border-radius: 8px;
  width: 100%;
  background-color: #444; /* Darker background for input */
  color: #f0f0f0; /* Light text color */
}

#done {
  background-color: #00d1d1; /* Bright teal button */
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s;
}

#delete {
  background-color: #ff4b4b; /* Bright red for delete button */
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s;
}

#done:disabled {
  background-color: #555; /* Dimmed button when disabled */
  cursor: not-allowed;
}

button:hover:enabled {
  background-color: #1b7575; /* Slightly darker teal on hover */
}

button#delete:hover:enabled {
  background-color: #cc3b3b; /* Darker red on hover */
}

/* Animations for adding and removing todos */
.fade-enter-active, .fade-leave-active {
  transition: all 0.5s ease;
}

.fade-enter {
  opacity: 0;
  transform: translateY(20px);
}

.fade-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}
</style>
