<script setup lang="ts">
  import { useId, watch, computed, useSlots, onMounted } from '#imports';
  import { ref } from 'vue'

  type Props = {
    variant?      : 'filled'
    shadow?       : boolean
    rounded?      : boolean | 'md' | 'lg' | 'xl' | 'full'
    noTransition? : boolean
    type?         : 'button' | 'submit' | 'reset'
    block?        : boolean
    uppercase?    : boolean
  }

  const slots = useSlots()
  const props = withDefaults(defineProps<Props>(), {
    variant   : 'filled',
    shadow    : true,
    rounded   : 'full',
    type      : 'button',
    block     : true,
    uppercase : true
  })
  const roundedClassName = computed(() => {
    if(props.rounded) {
      if(props.rounded === true) {
        return `rounded-lg`
      }
      return `rounded-${props.rounded}`
    }
  })
</script>

<template>
  <button
    :class="[
      'nc-button',

      // shadow style.
      shadow
        ? 'shadow'
        : null,

      // block style.
      block
        ? 'block'
        : null,

      // rounded styles.
      rounded
        ? roundedClassName
        : null,

      // uppercase style.
      uppercase
        ? 'uppercase'
        : null
    ]"
  >
    <div class="prepend-icon">
      <slot name="prepend-icon" />
    </div>

    <div class="text">
      <slot />
    </div>

    <div class="append-icon">
      <slot name="append-icon" />
    </div>
  </button>  
</template>

<style scoped>
  button {
    border: none;
    outline: none;
    background: none;
    cursor: pointer;
    background-color: #0891b2;
    color: #FEFEFE;
    transition: background-color 0.3s ease;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 0.5rem;
    min-width: fit-content;
  }
  button:active {
    background-color: #0e7490;
  }
  button:not(.block) {
    width: fit-content;
  }
  button.block {
    width: 100%;
  }
  button.uppercase {
    text-transform: uppercase;
  }
  .text {
    flex: 0;
  }
  .prepend-icon,
  .append-icon {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
