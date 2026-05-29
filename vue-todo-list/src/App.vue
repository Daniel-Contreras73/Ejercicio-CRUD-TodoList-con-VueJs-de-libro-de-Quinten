<template>
  <div id="app">
    <div class="todo-container">
      <header class="todo-header">
        <h1>📝 Todo List</h1>
        <p class="subtitle">Stay organized, stay focused</p>
      </header>

      <div class="todo-input-section">
        <input
          v-model="newTask"
          @keyup.enter="addTask"
          type="text"
          placeholder="Add a new task..."
          class="todo-input"
        />
        <button @click="addTask" class="add-btn">Add</button>
      </div>

      <div class="todo-filters">
        <button
          v-for="filter in filters"
          :key="filter"
          @click="currentFilter = filter"
          :class="['filter-btn', { active: currentFilter === filter }]"
        >
          {{ filter }}
        </button>
      </div>

      <ul class="todo-list" v-if="filteredTasks.length > 0">
        <li
          v-for="task in filteredTasks"
          :key="task.id"
          :class="['todo-item', { completed: task.done }]"
        >
          <input
            type="checkbox"
            :checked="task.done"
            @change="toggleTask(task.id)"
            class="todo-checkbox"
          />
          <span class="todo-text">{{ task.text }}</span>
          <button @click="removeTask(task.id)" class="delete-btn">✕</button>
        </li>
      </ul>

      <p v-else class="empty-state">No tasks here. Add one above!</p>

      <footer class="todo-footer" v-if="tasks.length > 0">
        <span>{{ pendingCount }} task(s) remaining</span>
        <button
          v-if="completedCount > 0"
          @click="clearCompleted"
          class="clear-btn"
        >
          Clear completed
        </button>
      </footer>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
  name: 'App',
  setup() {
    const newTask = ref('')
    const tasks = ref([])
    const currentFilter = ref('All')
    const filters = ['All', 'Active', 'Completed']

    const filteredTasks = computed(() => {
      if (currentFilter.value === 'Active') {
        return tasks.value.filter(t => !t.done)
      } else if (currentFilter.value === 'Completed') {
        return tasks.value.filter(t => t.done)
      }
      return tasks.value
    })

    const pendingCount = computed(() => tasks.value.filter(t => !t.done).length)
    const completedCount = computed(() => tasks.value.filter(t => t.done).length)

    function addTask() {
      const text = newTask.value.trim()
      if (!text) return
      tasks.value.push({
        id: Date.now(),
        text,
        done: false
      })
      newTask.value = ''
    }

    function toggleTask(id) {
      const task = tasks.value.find(t => t.id === id)
      if (task) task.done = !task.done
    }

    function removeTask(id) {
      tasks.value = tasks.value.filter(t => t.id !== id)
    }

    function clearCompleted() {
      tasks.value = tasks.value.filter(t => !t.done)
    }

    return {
      newTask,
      tasks,
      currentFilter,
      filters,
      filteredTasks,
      pendingCount,
      completedCount,
      addTask,
      toggleTask,
      removeTask,
      clearCompleted
    }
  }
}
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
  margin-bottom: 16px;
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

.todo-filters {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}

.filter-btn {
  flex: 1;
  padding: 8px;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  background: white;
  color: #666;
  font-size: 0.85rem;
  cursor: pointer;
  transition: all 0.2s;
}

.filter-btn.active {
  border-color: #667eea;
  background: #667eea;
  color: white;
}

.todo-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 20px;
}

.todo-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 14px 16px;
  background: #f8f9ff;
  border-radius: 10px;
  transition: opacity 0.2s;
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
  color: #aaa;
}

.todo-checkbox {
  width: 18px;
  height: 18px;
  cursor: pointer;
  accent-color: #667eea;
}

.todo-text {
  flex: 1;
  font-size: 0.95rem;
  color: #2d2d2d;
}

.delete-btn {
  background: none;
  border: none;
  color: #ccc;
  font-size: 1rem;
  cursor: pointer;
  padding: 2px 6px;
  border-radius: 4px;
  transition: color 0.2s;
}

.delete-btn:hover {
  color: #e53e3e;
}

.empty-state {
  text-align: center;
  color: #bbb;
  padding: 30px 0;
  font-size: 0.95rem;
}

.todo-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 16px;
  border-top: 1px solid #f0f0f0;
  color: #888;
  font-size: 0.85rem;
}

.clear-btn {
  background: none;
  border: none;
  color: #e53e3e;
  cursor: pointer;
  font-size: 0.85rem;
  text-decoration: underline;
}
</style>
