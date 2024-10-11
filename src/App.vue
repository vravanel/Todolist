<template>
  <h1>TodoList</h1>
  <form action="" @submit.prevent="newTask">
    <input type="text" placeholder="ajouter une nouvelle tâche" v-model="task">
    <button>Ajouter</button>
  </form>

  <div v-for="item in sortedTodos()" :key="item.title" v-if="todolist.length > 0">
    <input type="checkbox" v-model="item.completed" :id="item.title">
    <label :for="item.title" :class="{ completed: item.completed }">{{ item.title }}</label>
  </div>
  
  <div v-else>
    <p>Pas de liste</p>
  </div>
  
  <input type="checkbox" v-model="hideCompleted">
  <label>
    Masquer les tâches complétées
  </label>
</template>

<script setup>
import {ref} from 'vue'

const todolist = ref([]);
const task = ref('');
const hideCompleted = ref(false);

const newTask = () => {
  todolist.value.push({ title: task.value, completed: false, date: Date.now() });
  task.value = '';
}

const sortedTodos = () => {
  const sortedTodolist = todolist.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1);
  if (hideCompleted.value === true) {
    return sortedTodolist.filter(t => !t.completed);
  }
  return sortedTodolist;
}
</script>

<style>
.completed {
  opacity: .5;
  text-decoration: line-through;
}
</style>
