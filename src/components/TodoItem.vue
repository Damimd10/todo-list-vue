<script setup>
  defineProps({
    item: Object
  });

  defineEmits(['remove']);
</script>

<template>
  <label>
    <input type="checkbox" v-model="item.done" />
    <span :class="`bubble ${item.category}`"></span>
    <div class="todo-content">
      <input type="text" v-model="item.content" />
    </div>
    <div class="actions">
      <button class="delete" @click="$emit('remove', item)">Delete</button>
    </div>
  </label>
</template>