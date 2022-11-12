<script setup>
  defineProps({
    modelValue: String
  })

  defineEmits(['update:modelValue'])
</script>
<template>
   <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" placeholder="Name here" :value="modelValue" @input="$emit('update:modelValue', $event.target.value)" />
      </h2>
    </section>
</template>