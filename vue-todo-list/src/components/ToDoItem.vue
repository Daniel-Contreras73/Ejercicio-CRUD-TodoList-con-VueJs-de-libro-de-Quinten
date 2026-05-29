<template>
  <li class="todo-item" :class="{ completed: item.checked }">
    <input
      type="checkbox"
      :checked="item.checked"
      @change="$emit('toggle', item.id)"
      class="todo-checkbox"
    />
    <span class="todo-text">{{ item.label }}</span>
  </li>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue'
import type { TodoItem } from '../types/todo'

export default defineComponent({
  name: 'ToDoItem',
  props: {
    item: {
      type: Object as PropType<TodoItem>,
      required: true
    }
  },
  emits: ['toggle']
})
</script>

<style scoped>
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
</style>
