<template>
  <div class="todo-area">
    <div class="todo-header">
      <h2>Todo List</h2>
      <button class="add-btn" @click="showModal = true" aria-label="Добавить">
        <img src="/Component36.png" alt="Добавить" />
      </button>
    </div>

    <!-- Модальное окно -->
    <div v-if="showModal" class="modal-overlay" @click.self="showModal = false">
      <div class="modal">
        <button class="modal-close" @click="showModal = false" aria-label="Закрыть">
          <img src="/Component36-2.png" alt="Закрыть" />
        </button>
        <h3>Создать новую задачу</h3>
        <p>Описание</p>
        <input
          v-model="modalInput"
          placeholder="Введите описание"
          @keyup.enter="createTodo"
        />
        <div class="modal-actions">
          <button @click="createTodo" class="create-btn">Создать</button>
        </div>
      </div>
    </div>

    <form @submit.prevent="addTodo" class="todo-form">
      <img src="/Vector.png" alt="" />
      <input type="text" v-model="newTodo" placeholder="Поиск Имени, статуса или даты" />
    </form>

    <div class="todo-desc">
      <span>
        <img src="/Rectangle7614.png" alt="" />
        Описание
      </span>
      <div class="todo-desc-right">
        <span>
          <img src="/Rectangle7614.png" alt="" />
          Статус
        </span>
        <span>
          <img src="/Rectangle7614.png" alt="" />
          Дата
        </span>
      </div>
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
        <button @click="removeTodo(index)" aria-label="Удалить">🗑️</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const showModal = ref(false)
const modalInput = ref('')
const newTodo = ref('')
const todos = ref([])

function addTodo() {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value, done: false })
    newTodo.value = ''
  }
}

function removeTodo(index) {
  todos.value.splice(index, 1)
}

function createTodo() {
  if (modalInput.value.trim()) {
    todos.value.push({ text: modalInput.value, done: false })
    modalInput.value = ''
    showModal.value = false
  }
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

.add-btn {
  position: absolute;
  top: 104px;
  left: 1260px;
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.todo-form {
  display: flex;
  align-items: center;
  gap: 8px;
  position: absolute;
  top: 170px;
  left: 74px;
}

input[type="text"] {
  flex: 1;
  padding: 8px;
  font-size: 16px;
  border: none;
  outline: none;
  background: white;
}

.sort-block {
  position: absolute;
  top: 170px;
  left: 1100px;
  display: flex;
  align-items: center;
  gap: 8px;
  white-space: nowrap;
  font-size: 16px;
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
  margin-left: 8px;
  font-size: 20px;
  background: none;
  border: none;
  cursor: pointer;
}

.todo-desc {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  max-width: 1150px;
  margin: 0 auto;
  position: absolute;
  top: 220px;
  padding: 0 100px;
  font-size: 16px;
  background: white;
  z-index: 1;
  /* min-height: 32px; */
}

.todo-desc span {
  display: flex;
  align-items: center;
  gap: 8px;
}

.todo-desc img {
  width: 1px;
  height: 32px;
  object-fit: contain;
  margin-top: -6px;
}

.todo-desc-right {
  display: flex;
  align-items: center;
  gap: 32px;
}

.modal-overlay {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal {
  position: relative;
  min-width: 400px;
  padding: 32px 24px;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 4px 24px rgba(0,0,0,0.15);
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.modal-close {
  position: absolute;
  top: 12px;
  right: 12px;
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 4px;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-close img {
  width: 20px;
  height: 20px;
}

.modal-actions {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin-top: 16px;
}

.create-btn {
  background: rgba(240, 245, 255, 1);
  color: rgba(49, 75, 153, 1);
  border: none;
  padding: 12px 40px;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
}

.cancel-btn {
  background: #eee;
  color: #333;
  border: none;
  padding: 8px 20px;
  border-radius: 8px;
  font-weight: 400;
  cursor: pointer;
}

.modal h3,
.modal p {
  text-align: left;
  margin-left: 0;
}
</style>
