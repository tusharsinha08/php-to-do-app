<template>
  <div class="input-container">
    <input
      type="text"
      v-model="newTodo"
      @keyup.enter="addTodo"
      placeholder="Add a task"
      class="todo-input"
    />
    <button @click="addTodo" class="add-button">Add</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const newTodo = ref('')
const emit = defineEmits(['add-todo'])

const addTodo = () => {
  if (newTodo.value.trim() === '') return
  emit('add-todo', newTodo.value)
  newTodo.value = ''
}
</script>

<style scoped>
/* Container for input and button */
.input-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 2rem;
  margin-left: 2rem;
}

/* Styling for the input */
.todo-input {
  padding: 0.75rem;
  border: 2px solid #555; /* Darker border for input */
  border-radius: 8px;
  margin-right: 0.5rem;
  background-color: #333333; /* Dark background matching the todo items */
  color: #f0f0f0; /* Light text color for better contrast */
  font-family: 'Poppins', sans-serif;
  font-size: 1rem;
  width: 300px; /* Slightly larger width for the input */
  transition: border-color 0.3s ease;
}

.todo-input:focus {
  outline: none;
  border-color: #00d1d1; /* Teal border color on focus */
}

/* Styling for the Add button */
.add-button {
  padding: 0.75rem 1.5rem;
  background-color: #00d1d1; /* Teal background matching the theme */
  color: #ffffff; /* White text color for contrast */
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-family: 'Poppins', sans-serif;
  font-size: 1rem;
  font-weight: 600;
  transition: background-color 0.3s ease, transform 0.2s ease; /* Smooth transition for hover */
}

.add-button:hover {
  background-color: #008b8b; /* Darker teal on hover */
  transform: scale(1.05); /* Slight scaling effect on hover */
}

.add-button:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(0, 209, 209, 0.3); /* Teal focus outline */
}
</style>
