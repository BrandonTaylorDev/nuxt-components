<script setup>
  const x = ref('')
</script>

<template>
  <div class="p">
    <nc-text-field v-model="x" label="Label" />
    {{ x }}
  </div>
</template>

<style>
  .p {
    padding: 1rem;
  }
</style>
