<script setup>
import { ref } from 'vue';
let value = ref('')
let todos = ref([{
  isCompleted: false,
  text: 'Learn Vue'
}])
function addTodo() {
  todos.value.push({
    isCompleted: false,
    text: value.value
  })
  value.value = ''
}
function deleteTodo(index) {
  todos.value.splice(index, 1)
}
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
