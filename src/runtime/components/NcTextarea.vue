<script setup lang="ts">
  import { useId, watch, computed, useSlots } from '#imports';
  import { ref } from 'vue'

  type Props = {
    label?        : string
    modelValue?   : string
    variant?      : 'underlined' | 'filled'
    shadow?       : boolean
    rounded?      : 'md' | 'lg' | 'xl'
    noTransition? : boolean
    autocomplete? : string
    autofocus?    : boolean
  }
  const props = withDefaults(defineProps<Props>(), {
    label         : '',
    variant       : 'filled',
    shadow        : true,
    rounded       : 'lg',
    type          : 'text',
    autocomplete  : 'off',
    autofocus     : false
  })
  const slots   = useSlots()
  const emits   = defineEmits([ 'update:modelValue' ])
  const value   = ref(props?.modelValue ?? '')
  const focused = ref(false)
  const hasSlot = (name: string) => !!slots[name]

  // update Vue with our new value.
  watch(value, () => emits('update:modelValue', value.value))

  // update our value with a new prop value on prop update.
  watch(() => props.modelValue, () => value.value = props.modelValue ?? '', { immediate: true })
</script>

<template>
  <div
    :class="[
      'nc-textarea',

      // variant styles.
      `variant-${variant}`,

      // rounded styles.
      rounded && variant !== 'underlined'
        ? `rounded-${rounded}`
        : null,
    ]"
  >
    <div
      v-if="hasSlot('prepend-icon')"
      :class="[
        'prepend-icon-wrap',
      ]"
    >
      <slot name="prepend-icon" />
    </div>

    <div
      :class="[
        'controls'
      ]"
    >
      <textarea
        v-model="value"
        @focusin="focused = true"
        @focusout="focused = false"
        :class="[
          hasSlot('prepend-icon')
            ? 'has-prepend-icon'
            : null,
          hasSlot('append-icon')
            ? 'has-append-icon'
            : null,
          label
            ? 'has-label'
            : null
        ]"
      />

      <label
        v-if="label"
        :class="[
          hasSlot('prepend-icon')
            ? 'has-prepend-icon'
            : null,
          hasSlot('append-icon')
            ? 'has-append-icon'
            : null,
          focused || value != ''
            ? 'focused'
            : null
        ]"
      >
        {{ label }}
      </label>
    </div>

    <div
      v-if="hasSlot('append-icon')"
      :class="[
        'append-icon-wrap',
      ]"
    >
      <slot name="prepend-icon" />
    </div>
  </div>
</template>

<style scoped>

  /* the parent width is dependent on the textarea itself. */
  .nc-textarea {
    width: fit-content;
    position: relative;
    background-color: #FFFFFF;
    color: #6b7280;
    overflow: hidden;
  }

  .nc-textarea.variant-filled {
    box-shadow: var(--shadow);
  }
  .nc-textarea.variant-underlined {
    border-bottom: 2px #6b7280 solid;
  }
  
  .prepend-icon-wrap,
  .append-icon-wrap {
    position: absolute;
    width: 3rem;
    height: 100%;
    top: 0.5rem;
    pointer-events: none;
    display: flex;
    justify-content: center;
    align-items: flex-start;
  }

  .prepend-icon-wrap {
    left: 0;
  }

  .append-icon-wrap {
    right: 0;
  }

  textarea {
    margin: 0;
    padding: 1rem;
    display: block;
    min-width: 16rem;
    min-height: 4rem;
    width: 32rem;
    height: 8rem;
    border: none;
  }
  textarea:focus {
    outline: none;
  }

  textarea.has-prepend-icon {
    padding-left: 3rem;
  }

  textarea.has-append-icon {
    padding-right: 3rem;
  }

  textarea.has-label {
    padding-top: 1.125rem;
  }

  textarea:focus ~ label, .focused {
    top: 0.25rem;
    font-size: 0.75rem;
  }

  label {
    position: absolute;
    top: 0.5rem;
    left: 1rem;
    transition: top 0.3s ease, font-size 0.3s ease;
    user-select: none;
    pointer-events: none;
    background: lightblue;
    width: 100%;
  }
  label.has-prepend-icon {
    left: 3rem;
  }
  label.has-append-icon {
    right: 3rem;
    width: calc(100% - 6rem);
  }
  label.has-prepend-icon,
  label.has-append-icon:not(.has-prepend-icon) {
    width: calc(100% - 3rem);
  }

  .controls,
  textarea,
  label {
    background-color: inherit;
  }

  @media screen and (prefers-color-scheme: dark) {
    .nc-textarea {
      background-color: #4b5563;
    }

    textarea {
      color: #f9fafb;
    }

    label, .prepend-icon-wrap, .append-icon-wrap {
      color: #d1d5db;
    }
  }
</style>
