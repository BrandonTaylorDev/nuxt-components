<script setup>
  const x = ref('')
  const y = ref('')
  const showPass = ref(false)
</script>

<template>
  <nc-card class="p">
    <nc-text-field
      v-model="x"
      label="E-mail"
      type="email"
      variant="filled"
    >
      <template #prepend-icon>
        <icon name="mdi:email" size="2em" />
      </template>
    </nc-text-field>

    <nc-text-field
      v-model="y"
      label="Password"
      :type="showPass ? 'text' : 'password'"
      variant="filled"
    >
      <template #prepend-icon>
        <icon name="mdi:password" size="2em" />
      </template>

      <template #append-icon>
        <icon :name="showPass ? 'mdi:eye-off' : 'mdi:eye'" size="2em" @click="showPass = !showPass" />
      </template>
    </nc-text-field>

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
