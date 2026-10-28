<script setup lang="ts">
import { computed, useId, watch, ref } from 'vue'

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

const emit = defineEmits<{
  'value-change': [value: string | number]
  'error-validation': [fieldError: string | null]
}>()

watch(model, (newValue) => emit('value-change', newValue))

function clear() {
  model.value = ''
}

const fieldError = computed<string | null>(() => {
  const value = model.value

  if (props.type === 'text') {
    const value_str = value.toString()
    if (value_str.length < props.minLength) {
      return `Minimum length is ${props.minLength}, you entered ${value_str.length}`
    }
  }
  if (props.type === 'number') {
    if (value === '') {
      return `Value is expected`
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

watch(fieldError, (newError) => emit('error-validation', newError), { immediate: true })

const touched = ref(false)
</script>
<template>
  <div class="field">
    <label :for="id"><slot>Fallback text</slot></label>
    <input
      class="row"
      :id="id"
      v-model="model"
      :type="type"
      :class="{ highlighted: fieldError && touched }"
      @blur="touched = true"
    />
    <button class="row" @click="clear" type="button">Clear</button>
    <p class="error" v-if="fieldError !== null && touched">{{ fieldError }}</p>
  </div>
</template>
<style scoped>
.highlighted {
  border-color: red;
}
</style>
