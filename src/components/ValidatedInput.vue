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
  const value = model.value

  if (props.type === 'text') {
    const value_str = value.toString()
    if (value_str.length < props.minLength) {
      return `Minimun length is ${props.minLength}`
    }
  }
  if (props.type === 'number') {
    if (value === '') {
      return null
    }
    const value_num = Number(value)
    if (props.min !== null && value_num < props.min) {
      return `Min value is ${props.min}, you entered ${value_num}`
    }
    if (props.max !== null && value_num > props.max) {
      return `Max value is ${props.max}, you entered ${value_num}`
    }
  }
  return null
})
</script>
<template>
  <label :for="id"><slot>Fallback text</slot></label>
  <input :id="id" v-model="model" :type="type" :class="{ highlighted: fieldError }" />
  <button @click="clear" type="button">Clear</button>
  <p v-if="fieldError !== null">{{ fieldError }}</p>
</template>
<style scoped>
.highlighted {
  border-color: red;
}
</style>
