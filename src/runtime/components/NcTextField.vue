<script setup lang="ts">
  import { useId, watch, computed, useSlots, onMounted } from '#imports';
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

  const slots = useSlots()
  const props = withDefaults(defineProps<Props>(), {
    label   : '',
    variant : 'filled',
    shadow  : true,
    rounded : 'lg',
    type    : 'text',
  })
  const emits = defineEmits([ 'update:modelValue' ])
  const hasSlot = (name: string) => !!slots[name]

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
  const prependIconHasEvent = ref(false)
  const appendIconHasEvent = ref(false)
  
  // Update `value` when `props.modelValue` changes
  watch(() => props.modelValue, (newValue) => value.value = newValue)

  // Emit `update:modelValue` event when `value` changes,
  // so that parent component can update the bound value.
  watch(() => value.value, (newValue) => emits('update:modelValue', newValue))

  onMounted(() => {
    const prependIconSlot = slots['prepend-icon']?.()[0];
    const appendIconSlot = slots['append-icon']?.()[0];

    if(prependIconSlot?.props?.onClick) {
      prependIconHasEvent.value = true
    }

    if(appendIconSlot?.props?.onClick) {
      appendIconHasEvent.value = true
    }
  })
</script>

<template>
  <div
    :class="[
      'nc-text-field',

      // shadow styles.
      shadow  && variant !== 'underlined'
        ? 'shadow'
        : null,

      // rounded styles.
      rounded && variant !== 'underlined'
        ? roundedClassName
        : null,

      // transition styles.
      !noTransition
        ? 'transition'
        : null,
    ]"
  >
    <button
      v-if="hasSlot('prepend-icon')"
      :class="[
        'prepend-icon-wrap',
        prependIconHasEvent
          ? 'has-event'
          : null,

        !noTransition
          ? 'transition'
          : null,
      ]"
    >
      <slot name="prepend-icon" />
    </button>

    <div
      :class="[
        'controls'
      ]"
    >
      <input
        :id="inputId"
        v-model="value"
        @focusin="focused = true"
        @focusout="focused = false"
        :class="[
          variant,
          label
            ? 'has-label'
            : null,
          !noTransition
            ? 'transition'
            : null,
          
          hasSlot('prepend-icon')
            ? 'has-prepend-icon'
            : null
        ]"
        :aria-labelledby="labelId"
        :type="type"
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
            : null,
          
          hasSlot('prepend-icon')
            ? 'has-prepend-icon'
            : null
        ]"
      >
        {{ props.label }}
      </label>
    </div>

    <button
      v-if="hasSlot('append-icon')"
      :class="[
        'append-icon-wrap',
        appendIconHasEvent
          ? 'has-event'
          : null,
          
        !noTransition
          ? 'transition'
          : null,
      ]"
    >
      <slot name="append-icon" />
    </button>
  </div>
</template>

<style scoped>
  .nc-text-field {
    position: relative;
    height: 2rem;
    display: flex;
    background-color: #FFFFFF;
    min-width: 18rem;
  }

  .nc-text-field > div {
    height: 100%;
    border-radius: inherit;
    background-color: inherit;
  }

  .prepend-icon-wrap,
  .append-icon-wrap {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0;
    width: 3rem;
    background: none;
    color: inherit;
    border: none;
    font: inherit;
    outline: inherit;
    color: #6b7280
  }
  .prepend-icon-wrap.has-event,
  .append-icon-wrap.has-event {
    cursor: pointer;
  }

  .controls {
    flex-grow: 1;
  }

  .transition {
    transition: top 0.3s ease, transform 0.3s ease, font-size 0.3s ease, background-color 0.3s ease, color 0.3s ease;
  }

  label.has-prepend-icon {
    left: 3rem;
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
  input:not(:has(+ .has-prepend-icon)) {
    padding: 0 0.5rem;
  }
  
  input:focus ~ label, .focused {
    top: 0;
    transform: translateY(0);
    font-size: 0.75rem;
  }

  @media screen and (prefers-color-scheme: dark) {
    .nc-text-field {
      background-color: #4b5563
    }

    input {
      background-color: inherit;
      color: #f9fafb;
    }

    label, .prepend-icon-wrap, .append-icon-wrap {
      color: #d1d5db;
    }
  }
</style>
