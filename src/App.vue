<script setup>
  import { computed, onMounted, ref, watch } from 'vue';

  import Greeting from './components/Greeting.vue';
  import TodoForm from './components/TodoForm.vue';
  import TodoList from './components/TodoList.vue';

  const todos = ref([]);
  const name = ref('');

  const inputContent = ref('');
  const inputCategory = ref(null);

  const todosFilterByAsc = computed(() => todos.value.sort((a, b) => b.createdAt - a.createdAt));

  const resetForm = () => {
    inputContent.value = '';
    inputCategory.value = null;
  }

  const addTodo = () => {
    const isContentValid = inputContent.value.trim() !== '';
    const isCategoryValid = !inputCategory.value;

    if (isContentValid && isCategoryValid) return;

    todos.value.push({
      content: inputContent.value,
      category: inputCategory.value,
      createdAt: new Date().getTime(),
      done: false
    });

    resetForm();
  }
  
  const removeTodo = (todo) => {
    todos.value = todos.value.filter(currentTodo => currentTodo !== todo);
  }

  onMounted(() => {
    name.value = localStorage.getItem('name') || '';
    todos.value = JSON.parse(localStorage.getItem('toods')) || [];
  });

  watch(name, (value) => localStorage.setItem('name', value));

  watch(todos, (value) => localStorage.setItem('toods', JSON.stringify(value)), { deep: true });
</script>

<template>
  <main class="app">
   <greeting v-model="name" />
    <todo-form @submit="addTodo" v-model:content="inputContent" v-model:category="inputCategory" />
    <todo-list :todos="todosFilterByAsc" @remove="removeTodo" />
  </main>
</template>