<script setup lang="ts">
import { computed, useId } from 'vue'

const id = useId()

interface Props {
  type?: 'text' | 'number'
  minLength?: number
  min?: number | null
  max?: number | null
}
const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  minLength: 0,
  min: null,
  max: null,
})
const model = defineModel<string | number>({ default: '' })

function clear() {
  model.value = ''
}

const fieldError = computed<string | null>(() => {
  let value = model.value

  if (props.type === 'text') {
    value = value.toString()
    if (value.length < props.minLength) {
      return `Minimun length is ${props.minLength}`
    }
  }
  if (props.type === 'number') {
    value = Number(value)
    if (props.min !== null && value < props.min) {
      return `Min value is ${props.min}`
    }
    if (props.max !== null && value > props.max) {
      return `Max value is ${props.max}`
    }
  }
  return null
})
</script>
<template>
  <label :for="id"><slot>Fallback text</slot></label>
  <input :id="id" v-model="model" :type="type" />
  <button @click="clear" type="button">Clear</button>
  <p>{{ fieldError }}</p>
</template>
