<script setup>
  defineProps({
    category: String,
    content: String
  });

  defineEmits(['submit', 'update:category', 'update:content']);
</script>

<template>
  <section class="create-todo">
    <h3>CREATE TODO</h3>
    <form @submit.prevent="$emit('submit')">
      <h4>What's on your todo list?</h4>
      <input placeholder="e.g. make a video" type="text" :value="content" @input="$emit('update:content', $event.target.value)" />
      <h4>Pick a category</h4>
      <div class="options">
        <label>
          <input id="category1" name="category" type="radio" value="business" :value="category" @input="$emit('update:category', $event.target.value)" />
          <span class="bubble business" />
          <div>Business</div>
        </label>
        <label>
          <input id="category2" name="category" type="radio" value="personal" :value="category" @input="$emit('update:category', $event.target.value)" />
          <span class="bubble personal" />
          <div>Personal</div>
        </label>
      </div>
      <input type="submit" value="Add Todo" />
    </form>
  </section>
</template>