<script setup>
  const x = ref('')
</script>

<template>
  <nc-card class="p">
    <nc-text-field label="Label" v-model="x" variant="filled" rounded="xl" />

    <nc-textarea
      label="Label"
      rounded="xl"
      variant="filled"
    >
      <template #prepend-icon>
        <icon name="mdi:message" size="2em" />
      </template>
    </nc-textarea>

    <nc-button style="min-height: 3rem">
      <template #prepend-icon>
        <icon name="mdi:login" size="1.5em" />
      </template>
      login
    </nc-button>
  </nc-card>
</template>

<style>
  .p {
    margin: 1rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
    max-width: 32rem;
  }

  .nc-text-field {
    min-height: 3rem;
    /* background-color: #f3f3f3!important; */
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
