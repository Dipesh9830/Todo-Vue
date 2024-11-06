<template>
  <div class="app">
    <h1>Vue To-Do App</h1>
    <TodoInput @addTask="addTask" />
    <TodoList
      :tasks="sortedTasks"
      @toggleComplete="toggleComplete"
      @deleteTask="deleteTask"

    />
  </div>
</template>

<script>
import { ref, computed, watch } from 'vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'

export default {
  components: { TodoInput, TodoList },
  setup () {
    const tasks = ref(JSON.parse(localStorage.getItem('tasks') || '[]'))

    watch(
      tasks,
      (newTasks) => {
        localStorage.setItem('tasks', JSON.stringify(newTasks))
      },
      { deep: true }
    )

    const addTask = (task, priority = 'low', dueDate = null) => {
      tasks.value.push({ id: Date.now(), text: task, completed: false, priority, dueDate })
    }

    const toggleComplete = (taskId) => {
      const task = tasks.value.find((t) => t.id === taskId)
      if (task) task.completed = !task.completed
    }

    const deleteTask = (taskId) => {
      tasks.value = tasks.value.filter((task) => task.id !== taskId)
    }

    const sortedTasks = computed(() => {
      return tasks.value.slice().sort((a, b) => {
        const priorities = { high: 3, medium: 2, low: 1 }
        return priorities[b.priority] - priorities[a.priority]
      })
    })

    return { tasks, addTask, toggleComplete, deleteTask, sortedTasks }
  }
}
</script>

<style scoped>
.app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 30px;
}
</style>
