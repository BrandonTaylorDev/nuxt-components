<script setup lang="ts">
  type Props = {
    label?      : string
    modelValue? : string
  }

  const props = withDefaults(defineProps<Props>(), {
    label: ''
  })
  const emits = defineEmits([ 'update:modelValue' ])

  const id = useId()
  const value = ref(props.modelValue)
  
  // Update `value` when `props.modelValue` changes
  watch(() => props.modelValue, (newValue) => {
    value.value = newValue
  })

  // Emit `update:modelValue` event when `value` changes,
  // so that parent component can update the bound value.
  watch(() => value.value, (newValue) => {
    emits('update:modelValue', newValue)
  })
</script>

<template>
  <div class="nc-text-field">
    <label
      :id="id"
    >
      {{ props.label }}
    </label>

    <input
      v-model="value"
    />
  </div>
</template>

<style scoped>
  .nc-text-field {
    background-color: red;
    position: relative;
    height: 2rem;
    box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  }

  .nc-text-field label {
    position: absolute;
    top: 50%;
    transform: translateY(-50%)
  }

  .nc-text-field input {
    width: 100%;
    height: 100%;

    border: none;
    margin: 0;
    padding: 0;
  }
</style>
