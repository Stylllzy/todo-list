<script setup>
import { computed, onMounted, ref, watch } from 'vue';

const newItem = ref('');
const todos = ref([]);

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return a.createdAt - b.createdAt
}))

watch(todos, (todos) => {
  localStorage.setItem('todos', JSON.stringify(todos))
}, { deep: true })

const addTodo = () => {
  if (newItem.value.trim() === '') {
    return
  }

  todos.value.push({
    id: crypto.randomUUID(),
    title: newItem.value,
    completed: false
  });
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo)
}

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

</script>

<template>
  <div>
    <form @submit.prevent="addTodo" class="new-item-form">
      <div class="form-row">
        <label htmlFor="item">New Item</label>
        <input v-model="newItem" type="text" id="item" />
        <button class="btn">Add</button>
      </div>
    </form>

    <h1>Todo List</h1>
    <ul class="list">
      <li v-for="todo in todos_asc" :key="todo.id">
        <label>
          <input type="checkbox" v-model="todo.completed" />
          <span>{{ todo.title }}</span>
        </label>
        <button class="btn btn-danger" @click="removeTodo(todo)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
