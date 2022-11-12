<script setup>
  import { computed, onMounted, ref, watch } from 'vue';

  const todos = ref([]);
  const name = ref('');

  const inputContent = ref('');
  const inputCategory = ref('');

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
    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>
    <section class="create-todo">
      <h3>CREATE TODO</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input placeholder="e.g. make a video" type="text" v-model="inputContent" />
        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input id="category1" name="category" type="radio" value="business" v-model="inputCategory" />
            <span class="bubble business" />
            <div>Business</div>
          </label>
          <label>
            <input id="category2" name="category" type="radio" value="personal" v-model="inputCategory" />
            <span class="bubble personal" />
            <div>Personal</div>
          </label>
        </div>
        <input type="submit" value="Add Todo" />
      </form>
    </section>
    <section class="todo-list">
      <h3>TOOO LIST</h3>
      <div class="list" id="todo-list">
        <div v-for="todo in todosFilterByAsc" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
            <div class="todo-content">
              <input type="text" v-model="todo.content" />
            </div>
            <div class="actions">
              <button class="delete" @click="removeTodo(todo)">Delete</button>
            </div>
          </label>
        </div>
      </div>
    </section>
  </main>
</template>