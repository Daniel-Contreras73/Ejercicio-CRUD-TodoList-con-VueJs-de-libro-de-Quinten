<template>
  <div id="app">
    <div class="todo-container">
      <header class="todo-header">
        <h1>Todo List</h1>
        <p class="subtitle">Stay organized, stay focused</p>
      </header>

      <div class="todo-input-section">
        <input
          v-model="newLabel"
          @keyup.enter="addItem"
          type="text"
          placeholder="Add a new task..."
          class="todo-input"
        />
        <button @click="addItem" class="add-btn">Add</button>
      </div>

      <ToDoList :items="items" @toggle="toggleItem" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue'
import type { TodoItem } from './types/todo'
import ToDoList from './components/ToDoList.vue'

const STORAGE_KEY = 'vue-todo-items'

export default defineComponent({
  name: 'App',
  components: { ToDoList },
  setup() {
    const stored = localStorage.getItem(STORAGE_KEY)
    const items = ref<TodoItem[]>(stored ? JSON.parse(stored) : [])
    const newLabel = ref('')

    watch(items, (val) => {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(val))
    }, { deep: true })

    function addItem() {
      const label = newLabel.value.trim()
      if (!label) return
      items.value.push({ id: Date.now(), label, checked: false })
      newLabel.value = ''
    }

    function toggleItem(id: number) {
      const item = items.value.find(i => i.id === id)
      if (item) item.checked = !item.checked
    }

    return { items, newLabel, addItem, toggleItem }
  }
})
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

#app {
  width: 100%;
  max-width: 520px;
}

.todo-container {
  background: white;
  border-radius: 16px;
  padding: 32px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
}

.todo-header {
  text-align: center;
  margin-bottom: 28px;
}

.todo-header h1 {
  font-size: 2rem;
  color: #2d2d2d;
  margin-bottom: 4px;
}

.subtitle {
  color: #888;
  font-size: 0.9rem;
}

.todo-input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.todo-input {
  flex: 1;
  padding: 12px 16px;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  font-size: 0.95rem;
  outline: none;
  transition: border-color 0.2s;
}

.todo-input:focus {
  border-color: #667eea;
}

.add-btn {
  padding: 12px 20px;
  background: #667eea;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s;
}

.add-btn:hover {
  background: #5a6fd6;
}
</style>
