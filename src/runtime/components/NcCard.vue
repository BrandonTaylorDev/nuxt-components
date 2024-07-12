<script setup lang="ts">
  import { computed } from '#imports';

  type Props = {
    shadow?       : boolean
    rounded?      : boolean | 'md' | 'lg' | 'xl' | 'full'
    noTransition? : boolean
  }

  const props = withDefaults(defineProps<Props>(), {
    shadow    : true,
    rounded   : 'lg',
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
  <div
    :class="[
      'nc-card',
      
      shadow
        ? 'shadow'
        : null,

      rounded
        ? roundedClassName
        : null
    ]"
  >
    <slot />
  </div>
</template>

<style scoped>
  .nc-card {
    background-color: #FFFFFF;
    padding: 1rem;
    min-width: fit-content;
  }

  @media screen and (prefers-color-scheme: dark) {
    .nc-card {
      background-color: #1f2937;
    }
  }
</style>
