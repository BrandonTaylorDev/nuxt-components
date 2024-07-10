<script setup lang="ts">
  import { useId } from '#imports';
  import { watch, computed } from '#imports'
  import { ref } from 'vue'

  type Props = {
    label?        : string
    modelValue?   : string
    variant?      : 'underlined' | 'filled'
    shadow?       : boolean
    rounded?      : boolean | 'md' | 'lg' | 'xl'
    noTransition? : boolean
    autocomplete? : boolean
    type?         : 'text' | 'password' | 'email'
  }

  const props = withDefaults(defineProps<Props>(), {
    label   : '',
    variant : 'filled',
    shadow  : true,
    rounded : 'lg',
    type    : 'text'
  })
  const emits = defineEmits([ 'update:modelValue' ])

  const inputId   = useId()
  const labelId   = useId()
  const value     = ref(props.modelValue)
  const focused   = ref(false)
  const roundedClassName = computed(() => {
    if(props.rounded) {
      if(props.rounded === true) {
        return `rounded-lg`
      }
      return `rounded-${props.rounded}`
    }
  })
  
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
  <div
    :class="[
      'nc-text-field',
      shadow && variant !== 'underlined' ? 'shadow' : null,
      rounded && variant !== 'underlined' ? roundedClassName : null,
      !noTransition ? 'transition' : null,
    ]"
  >
    <input
      :id="inputId"
      v-model="value"
      @focusin="focused = true"
      @focusout="focused = false"
      :class="[
        variant,
        label ? 'has-label' : null,
        !noTransition ? 'transition' : null
      ]"
      :aria-labelledby="labelId"
    />

    <label
      :id="labelId"
      :for="inputId"
      :class="[
        focused || value != ''
          ? 'focused'
          : null,
        !noTransition
          ? 'transition'
          : null
      ]"
    >
      {{ props.label }}
    </label>
  </div>
</template>

<style scoped>
  .nc-text-field {
    position: relative;
    height: 2rem;
  }

  .transition {
    transition: top 0.3s ease, transform 0.3s ease, font-size 0.3s ease, background-color 0.3s ease, color 0.3s ease;
  }

  .underlined {
    background-color: transparent;
    border-bottom: 2px #6b7280 solid;
  }

  label {
    position: absolute;
    top: 50%;
    left: 0.5rem;
    transform: translateY(-50%);
    color: #6b7280;
    user-select: none;
    pointer-events: none;
  }

  input {
    width: 100%;
    height: 100%;
    border: none;
    margin: 0;
    padding: 0;
    padding-left: 0.5rem;
    font-size: 0.9rem;
    border-radius: inherit;
    transition: inherit;
  }
  input:focus {
    outline: none;
  }
  input.has-label {
    padding-top: 0.5rem;
  }
  
  input:focus ~ label, .focused {
    top: 0;
    transform: translateY(0);
    font-size: 0.75rem;
  }

  @media screen and (prefers-color-scheme: dark) {
    input {
      background-color: #4b5563;
      color: #f9fafb;
    }

    label {
      color: #d1d5db;
    }
  }
</style>
