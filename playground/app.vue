<script setup>
  const x = ref('')
</script>

<template>
  <div class="p">
    <nc-text-field v-model="x" label="Label" style="min-height: 3rem" variant="filled" />
  </div>
</template>

<style>
  .p {
    padding: 1rem;
  }

  html {
    background-color: lightgray;
    transition: background-color 0.3s ease, color 0.3s ease;
  }

  @media screen and (prefers-color-scheme: dark) {
    html {
      background-color: #212121;
      color: #FEFEFE;
    }
  }
</style>
