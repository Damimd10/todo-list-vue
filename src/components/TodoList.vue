<script setup>
  import TodoItem from './TodoItem.vue';

  defineProps({
    todos: Array
  });

  defineEmits(['remove']);
</script>

<template>
  <section class="todo-list">
    <h3>TOOO LIST</h3>
    <div class="list" id="todo-list">
      <div v-for="todo in todos" :class="`todo-item ${todo.done && 'done'}`">
        <todo-item :item="todo" @remove="(item) => $emit('remove', item)"/>
      </div>
    </div>
  </section>
</template>