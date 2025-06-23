<template>
  <div class="todo-area">
  <div class="todo-header">
    <h2>Todo List</h2>
    <button @click="addTodo" aria-label="Добавить">
      <img src="/Component36.png" alt="">
    </button>
  </div>
  <form @submit.prevent="addTodo" class="todo-form">
    <img src="/Vector.png" alt="">
    <input type="text" v-model="newTodo" placeholder="Добавить задачу..." />
  </form>

  <div class="todo-desc">
    <span>
        <img src="/Rectangle7614.png" alt="">
        Описание
    </span>
    <span>
        <img src="/Rectangle7614.png" alt="">
        Статус
    </span>
    <span>
        <img src="/Rectangle7614.png" alt="">
        Дата
    </span>
  </div>

  <div class="sort-block">
    <span>Сортировать по: <b>Дата</b></span>
    <img src="/Vector9.png" alt="Сортировка" />
  </div>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <label style="flex: 1; display: flex; align-items: center; gap: 8px;">
          <input type="checkbox" v-model="todo.done" />
          <span :style="{ textDecoration: todo.done ? 'line-through' : 'none' }">
            {{ todo.text }}
          </span>
        </label>
        <button @click="removeTodo(index)" aria-label="Удалить">
          🗑️
        </button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const newTodo = ref('')
const todos = ref([])

function addTodo() {
  if (newTodo.value.trim() !== '') {
    todos.value.push({ text: newTodo.value, done: false })
    newTodo.value = ''
  }
}

function removeTodo(index) {
  todos.value.splice(index, 1)
}
</script>

<style scoped>
.todo-area {
  position: relative;
  max-width: 1300px;
  margin: 0 auto;
}

.todo-header {
  display: flex;
  align-items: center;
  gap: 8px;
  position: relative;
}

.todo-header h2 {
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  font-size: 24px;
  line-height: 132%;
  position: absolute;
  top: 104px;
  left: 40px;
  width: 113px;
  height: 32px;
  letter-spacing: 0%;
}

.todo-header button {
  position: absolute;
  top: 104px;
  left: 1260px; /* ваша позиция для кнопки */
}

.todo-form {
  display: flex;
  align-items: center;
  gap: 8px;
  position: absolute;
  top: 170px;
  left: 74px;
  margin: 0;
}

input[type="text"] {
  flex: 1;
  min-width: 0;
  padding: 8px;
  font-size: 16px;
  border: none !important;
  outline: none !important;
  box-shadow: none !important;
  background: white;
}

.sort-block {
  position: absolute;
  top: 170px;      /* под кнопкой, на уровне input */
  left: 1100px;    /* правее кнопки, подберите нужное значение */
  display: flex;
  align-items: center;
  gap: 8px;
  white-space: nowrap;
  font-size: 16px;
}

button {
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;

  position: absolute; 
  left: 1260px;
  top: 100px;
}

button:active {
  opacity: 0.7;
}

ul {
  position: absolute;
  top: 260px;
  left: 74px;
  width: calc(100% - 148px);
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  gap: 8px;
  margin: 8px 0;
}

li button {
  position: static;
  margin-left: 8px;
  align-self: center;
  font-size: 20px;
  line-height: 1;
  padding: 0;
}

.todo-desc {
  width: 100%;
  max-width: 1150px; /* как у .todo-area */
  margin: 0 auto;
  left: 0;
  right: 0;
  position: absolute;
  top: 220px;
  padding: 0 24px; /* уменьшите паддинги */
  box-sizing: border-box;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 16px;
  font-weight: 500;
  letter-spacing: 0.5px;
  background: white;
  z-index: 1;
  min-height: 32px;
}

.todo-desc span {
  display: flex;
  align-items: center;
  gap: 10px; /* расстояние между иконкой и текстом */
}

.todo-desc img {
  width: 1px;  /* или нужный размер */
  height: 32px;
  object-fit: contain;
  margin-top: -6px;
}
</style>