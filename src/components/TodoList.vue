<template>
    <ul>
      <li v-for="task in tasks" :key="task.id" :class="task.priority">
        <label>
          <input type="checkbox" :checked="task.completed" @change="toggleTask(task.id)" />
          <span :class="{ completed: task.completed }">{{ task.text }}</span>
        </label>
        <span v-if="task.dueDate" class="due-date">Due: {{ task.dueDate }}</span>
        <button @click="deleteTask(task.id)">Delete</button>
      </li>
    </ul>
  </template>

<script>
export default {
  props: ['tasks'],
  emits: ['toggleComplete', 'deleteTask'],
  methods: {
    toggleTask (taskId) {
      this.$emit('toggleComplete', taskId)
    },
    deleteTask (taskId) {
      this.$emit('deleteTask', taskId)
    }
  }
}
</script>

 <style scoped>
.low {
  color: green;
}
.medium {
  color: orange;
}
.high {
  color: red;
}
.due-date {
  font-style: italic;
  margin-left: 10px;
}
</style>
