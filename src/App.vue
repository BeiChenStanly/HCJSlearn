<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

let value = ref('');
let todos = ref([]);

// API URL
const apiUrl = 'http://192.168.1.7:3000/todos';

async function fetchTodos() {
  try {
    const response = await axios.get(apiUrl);
    todos.value = response.data;
  } catch (error) {
    console.error('Error fetching todos:', error);
  }
}

async function addTodo() {
  const newTodo = { isCompleted: false, text: value.value };
  try {
    await axios.post(apiUrl, newTodo);
    fetchTodos();
    value.value = '';
  } catch (error) {
    console.error('Error adding todo:', error);
  }
}

async function deleteTodo(index) {
  try {
    await axios.delete(`${apiUrl}/${todos.value[index].id}`);
    fetchTodos()
  } catch (error) {
    console.error('Error deleting todo:', error);
  }
}

// Fetch todos on component mount
onMounted(fetchTodos);
</script>


<template>
  <div class="todo-app">
    <h1 class="todo-title">Todo List</h1>
    <div class="todo-form">
      <input v-model="value" type="text" class="todo-input" placeholder="Enter your task here">
      <div @click="addTodo" class="todo-submit">Add</div>
    </div>
    <div class="todo-list">
      <div v-for="item, index in todos" :class="(item.isCompleted) ? 'todo-item completed' : 'todo-item'">
        <div>
          <input v-model="item.isCompleted" type="checkbox">
          <span class="todo-text">{{ item.text }}</span>
        </div>
        <div @click="deleteTodo(index)" class="todo-delete">del</div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
