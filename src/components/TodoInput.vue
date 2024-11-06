<template>
    <div>
      <input v-model="task" placeholder="Add a new task" @keyup.enter="submitTask" />
      <select v-model="priority">
        <option value="low">Low</option>
        <option value="medium">Medium</option>
        <option value="high">High</option>
      </select>
      <input type="date" v-model="dueDate" />
      <button @click="submitTask">Add Task</button>
    </div>
  </template>

<script>
import { ref } from 'vue'

export default {
  emits: ['addTask'],
  setup (props, { emit }) {
    const task = ref('')
    const priority = ref('low')
    const dueDate = ref(null)

    const submitTask = () => {
      if (task.value.trim()) {
        emit('addTask', task.value.trim(), priority.value, dueDate.value)
        task.value = ''
        priority.value = 'low'
        dueDate.value = null
      }
    }

    return { task, priority, dueDate, submitTask }
  }
}
</script>
