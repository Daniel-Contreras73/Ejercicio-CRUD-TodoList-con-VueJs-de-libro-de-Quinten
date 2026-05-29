<template>
  <ul class="todo-list" v-if="sortedItems.length > 0">
    <ToDoItem
      v-for="item in sortedItems"
      :key="item.id"
      :item="item"
      @toggle="$emit('toggle', $event)"
    />
  </ul>
  <p v-else class="empty-state">No tasks yet. Add one above!</p>
</template>

<script lang="ts">
import { defineComponent, computed, type PropType } from 'vue'
import type { TodoItem } from '../types/todo'
import ToDoItem from './ToDoItem.vue'

export default defineComponent({
  name: 'ToDoList',
  components: { ToDoItem },
  props: {
    items: {
      type: Array as PropType<TodoItem[]>,
      required: true
    }
  },
  emits: ['toggle'],
  setup(props) {
    const sortedItems = computed(() => [
      ...props.items.filter(item => !item.checked),
      ...props.items.filter(item => item.checked)
    ])
    return { sortedItems }
  }
})
</script>

<style scoped>
.todo-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 20px;
  padding: 0;
}

.empty-state {
  text-align: center;
  color: #bbb;
  padding: 30px 0;
  font-size: 0.95rem;
}
</style>
