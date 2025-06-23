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
          style="height: 40px; border-radius: 8px; border: rgba(221, 226, 228, 1) 1px solid; padding: 0px 15px; margin-top: -25px;"
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
      <input
        type="text"
        v-model="searchQuery"
        placeholder="Поиск Имени, статуса или даты"
      />
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
      <span>Сортировать по:</span>
      <select v-model="sortBy" class="sort-block-select">
        <option value="status">Статусу</option>
        <option value="date">Дате</option>
      </select>
    </div>

    <ul>
      <li v-for="todo in filteredAndSortedTodos" :key="todo.id" class="todo-item">
        <div class="todo-left">
          <span class="custom-checkbox" @click="toggleDone(todo.id)">
            <img
              :src="todo.done ? '/Group1427.png' : '/Ellipse34.png'"
              :style="todo.done
                ? 'width: 28px; height: 28px; margin-top: 7px'
                : 'width: 20px; height: 20px;'"
              alt="Статус"
            />
          </span>
          <span class="todo-text">
            {{ todo.text }}
          </span>
        </div>
        <div class="todo-right">
          <span class="todo-status" :class="todo.done ? 'done' : 'in-progress'">
            {{ todo.done ? 'Выполнено' : 'В работе' }}
          </span>
          <span class="todo-date">
            {{ formatDate(todo.createdAt) }}
          </span>
          <button @click="removeTodo(todo.id)" style="background: none; border: none;" aria-label="Удалить"><img src="/Component36-2.png" alt=""></button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const showModal = ref(false)
const modalInput = ref('')
const newTodo = ref('')
const todos = ref([])
const searchQuery = ref('')
const sortBy = ref('status')

let nextId = 1;

const filteredAndSortedTodos = computed(() => {
  let result = [...todos.value];
  if (searchQuery.value.trim()) {
    const query = searchQuery.value.trim().toLowerCase();
    result = result.filter(todo => {
      const textMatch = todo.text.toLowerCase().includes(query);
      const status = todo.done ? 'выполнено' : 'в работе';
      const statusMatch = status.includes(query);
      const dateMatch = todo.createdAt
        ? formatDate(todo.createdAt).includes(query)
        : false;
      return textMatch || statusMatch || dateMatch;
    });
  }
  if (sortBy.value === 'status') {
    result = [...result].sort((a, b) => a.done - b.done);
  } else if (sortBy.value === 'date') {
    result = [...result].sort((a, b) => {
      const dateA = a.createdAt ? new Date(a.createdAt) : new Date(0);
      const dateB = b.createdAt ? new Date(b.createdAt) : new Date(0);
      return dateB - dateA;
    });
  }
  return result;
});

function addTodo() {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value, done: false })
    newTodo.value = ''
  }
}

function removeTodo(id) {
  todos.value = todos.value.filter(t => t.id !== id);
}

function createTodo() {
  if (modalInput.value.trim() !== '') {
    todos.value.push({
      id: nextId++,
      text: modalInput.value,
      done: false,
      createdAt: new Date()
    })
    modalInput.value = ''
    showModal.value = false
  }
}

function formatDate(date) {
  if (!date) return ''
  const d = new Date(date)
  return d.toLocaleDateString('ru-RU')
}

function toggleDone(id) {
  const todo = todos.value.find(t => t.id === id);
  if (todo) todo.done = !todo.done;
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

.sort-block select {
  border: none;
  outline: none;
  background: transparent;
  font-size: 16px;
  cursor: pointer;
}

ul {
  position: absolute;
  top: 260px;
  left: 74px;
  width: 100%;
  list-style: none;
  padding: 0;

}

.todo-item {
  min-height: 58px;
  height: 58px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  padding: 8px 16px;
  background: #fff;
  transition: box-shadow 0.2s;
  margin-bottom: 1px;
  border-bottom: 1px solid rgba(238, 235, 233, 1);
}

.todo-item:hover {
  box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  background-color: rgba(246, 249, 255, 1);
}

.todo-left {
  display: flex;
  align-items: center;
  gap: 12px;
  min-width: 0;
}

.todo-text {
  flex: 1;
  min-width: 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.todo-right {
  display: flex;
  align-items: center;
  gap: 16px;
}

.custom-checkbox {
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.todo-status.done {
  color: #4caf50;
}
.todo-status.in-progress {
  color: #ff9800;
}

.todo-date {
  color: #888;
  font-size: 14px;
  min-width: 90px;
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
  background: rgba(0,0,0,0.2); /* затемнение */
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  backdrop-filter: blur(6px); /* вот это добавляет блюр */
  /* Для поддержки Safari: */
  -webkit-backdrop-filter: blur(6px);
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

.modal h3{
  text-align: left;
  margin-left: 0;
  font-size: 18px;
  font-weight: 700;
  line-height: 132%;
}
.modal p {
  text-align: left;
  margin-left: 0;
  font-size: 14px;
  margin-top: -10px;
}

.todo-status {
  color: #4caf50; /* зелёный для выполнено */
  font-size: 14px;
  min-width: 80px;
}

.todo-status::after {
  content: '';
  display: inline-block;
  margin-left: 4px;
}

.custom-checkbox img {
  width: 20px;
  height: 20px;
  object-fit: contain;
  display: block;
}

.checkbox-large {
  width: 32px;
  height: 32px;
}

.checkbox-small {
  width: 20px;
  height: 20px;
}
</style>
