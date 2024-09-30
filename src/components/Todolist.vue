<template>
  <ul>
    <li v-for="(todo, index) in todos" :key="index" class="todo-item">
      <div v-if="editingIndex !== index" @dblclick="editTodo(index)">
        <span>{{ todo }}</span>
        <button @click="removeTodo(index)" class="done-button">Done</button>
      </div>
      <div v-else>
        <input type="text" v-model="todos[index]" @keyup.enter="stopEditing" @blur="stopEditing" />
      </div>
    </li>
  </ul>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  todos: Array
})

const emit = defineEmits(['remove-todo'])
const editingIndex = ref(null)

const editTodo = (index) => {
  editingIndex.value = index
}

const stopEditing = () => {
  editingIndex.value = null
}

const removeTodo = (index) => {
  emit('remove-todo', index)
}
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
  font-family: 'Poppins', sans-serif;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #1f1f1f; /* Dark background for each todo item */
  margin: 15px 0;
  padding: 15px 20px;
  border-radius: 12px;
  border: 1px solid #333; /* Darker border to blend with the background */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3); /* Stronger shadow for a subtle lift */
  transition: box-shadow 0.2s ease, transform 0.2s ease;
}

.todo-item:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.5); /* Stronger hover shadow */
  transform: translateY(-2px);
}

.todo-item span {
  flex-grow: 1;
  color: #f4f4f9; /* Light text for better readability */
  font-weight: 500;
}

.todo-item input {
  padding: 10px;
  width: 100%;
  font-size: 1rem;
  color: #f4f4f9; /* Light text for input */
  background-color: #333; /* Darker input background */
  border: 1px solid #555; /* Dark border for the input */
  border-radius: 4px;
  outline: none;
  transition: border-color 0.2s ease;
}

.todo-item input:focus {
  border-color: #ff6b6b; /* Red border focus */
}

button.done-button {
  background-color: #4a9c59; /* Darker green */
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
  font-weight: 600;
}

button.done-button:hover {
  background-color: #ff4040; /* Darker red on hover */
}

button.done-button:focus,
input:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(255, 107, 107, 0.25); /* Subtle red focus outline */
}

body {
  background-color: #121212; /* Dark background for the whole app */
  color: #f4f4f9;
  font-family: 'Poppins', sans-serif;
}
</style>
